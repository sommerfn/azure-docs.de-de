---
title: Bewährte Methoden für den Azure Service Fabric-Anwendungsentwurf | Microsoft-Dokumentation
description: Bewährte Methoden für die Entwicklung von Service Fabric-Anwendungen.
services: service-fabric
documentationcenter: .net
author: markfussell
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/18/2019
ms.author: msfussell
ms.openlocfilehash: 30d696337061ade6b79c7ec0e4c4de67651f0dad
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203457"
---
# <a name="azure-service-fabric-application-design-best-practices"></a>Bewährte Methoden für den Azure Service Fabric-Anwendungsentwurf

Dieser Artikel enthält bewährte Methoden für das Erstellen von Anwendungen und Diensten in Azure Service Fabric.
 
## <a name="get-familiar-with-service-fabric"></a>Kennenlernen von Service Fabric
* Lesen Sie den Artikel [Sie möchten sich über Service Fabric informieren?](service-fabric-content-roadmap.md).
* Informieren Sie sich über [Service Fabric-Anwendungsszenarien](service-fabric-application-scenarios.md).
* Machen Sie sich mit den Auswahlmöglichkeiten für das Programmiermodell anhand der [Übersicht über das Service Fabric-Programmiermodell](service-fabric-choose-framework.md) vertraut.



## <a name="application-design-guidance"></a>Leitfaden für den Anwendungsentwurf
Machen Sie sich mit der [allgemeinen Architektur](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric) von Service Fabric-Anwendungen und ihren [Entwurfsüberlegungen](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#design-considerations) vertraut.

### <a name="choose-an-api-gateway"></a>Auswählen eines API-Gateways
Verwenden Sie einen API-Gatewaydienst, der mit Back-End-Diensten kommuniziert, die dann horizontal hochskaliert werden können. Die folgenden API-Gatewaydienste werden am häufigsten verwendet:

- [Azure API Management](https://docs.microsoft.com/azure/service-fabric/service-fabric-api-management-overview). Dieser Dienst ist [in Service Fabric integriert](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-deploy-api-management).
- [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) oder [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/) mit [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor) zum Lesen aus Event Hub-Partitionen.
- [Træfik-Reverseproxy](https://blogs.msdn.microsoft.com/azureservicefabric/2018/04/05/intelligent-routing-on-service-fabric-with-traefik/) mit dem [Azure Service Fabric-Anbieter](https://docs.traefik.io/configuration/backends/servicefabric/).
- [Azure Application Gateway:](https://docs.microsoft.com/azure/application-gateway/)

   > [!NOTE] 
   > Azure Application Gateway ist nicht direkt mit Service Fabric integriert. Azure API Management ist in der Regel die bevorzugte Option.
- Ihr eigenes benutzerdefiniertes [ASP.NET Core](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-aspnetcore)-Webanwendungsgateway.

### <a name="stateless-services"></a>Zustandslose Dienste
Wir empfehlen, dass Sie immer damit beginnen, zustandslose Dienste mit [Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction) zu erstellen und den Zustand in einer Azure-Datenbank, in Azure Cosmos DB oder Azure Storage zu speichern. Für die meisten Entwickler ist der externalisierte Zustand der vertrautere Ansatz. Dieser Ansatz ermöglicht auch die Nutzung von Abfragefunktionen im Speicher.  

### <a name="when-to-use-stateful-services"></a>Szenarien für zustandsbehaftete Dienste
Ziehen Sie zustandsbehaftete Dienste in Betracht, wenn Sie ein Szenario mit geringer Latenzzeit haben und die Daten in der Nähe des Computes halten müssen. Einige Beispielszenarien sind digitale IoT-Zwillingsgeräte, Spielstatus, Sitzungsstatus, Zwischenspeicherung von Daten aus einer Datenbank und zeitintensive Workflows zur Verfolgung von Aufrufen anderer Dienste.

Festlegen des Zeitrahmens für die Datenaufbewahrung:

- **Zwischengespeicherte Daten:** Sie verwenden Zwischenspeicherung, wenn die Latenz zu externen Speichern ein Problem darstellt. Verwenden Sie einen zustandsbehafteten Dienst als eigenen Datencache, oder verwenden Sie die [Open-Source-Option „SoCreate service-fabric-distributed-cache“](https://github.com/SoCreate/service-fabric-distributed-cache). In diesem Szenario müssen Sie sich keine Gedanken darüber machen, alle Daten im Cache zu verlieren.
- **Zeitgebundene Daten:** In diesem Szenario müssen Sie Daten aus Latenzgründen eine Zeit lang nah an der Computeressource halten, aber diese Daten dürfen in einem *Notfall* verloren gehen. So müssen beispielsweise in vielen IoT-Lösungen die Daten nah an der Computeressource sein, etwa bei der Berechnung der Durchschnittstemperatur der letzten Tage. Wenn diese Daten aber verloren gehen, dann sind die aufgezeichneten spezifischen Datenpunkte nicht so wichtig. Darüber hinaus ist in diesem Szenario in der Regel die Sicherung der einzelnen Datenpunkte nicht von Bedeutung. Sie sichern nur berechnete Durchschnittswerte, die in regelmäßigen Abständen in einen externen Speicher geschrieben werden.  
- **Langfristige Daten:** Zuverlässige Sammlungen können Ihre Daten dauerhaft speichern. In diesem Fall müssen Sie sich jedoch auf eine [Notfallwiederherstellung](https://docs.microsoft.com/azure/service-fabric/service-fabric-disaster-recovery) vorbereiten, einschließlich der [Konfiguration von Richtlinien für regelmäßige Sicherungen](https://docs.microsoft.com/azure/service-fabric/service-fabric-backuprestoreservice-configure-periodic-backup) für Ihre Cluster. Im Endeffekt konfigurieren Sie, was passiert, wenn Ihr Cluster in einem Notfall zerstört wird, wo Sie einen neuen Cluster erstellen müssen und wie Sie neue Anwendungsinstanzen bereitstellen und nach der letzten Sicherung wiederherstellen.

Sparen von Kosten und Verbessern der Verfügbarkeit:
- Mit zustandsbehafteten Diensten können Sie die Kosten senken, da Ihnen keine Datenzugriffs- und Transaktionskosten aus dem Remotespeicher entstehen und Sie keinen weiteren Dienst wie etwa Azure Cache for Redis verwenden müssen.
- Die Verwendung von zustandsbehafteten Diensten hauptsächlich für die Speicherung und nicht für Compute ist teuer und wird nicht empfohlen. Betrachten Sie zustandsbehaftete Dienste als Compute mit günstigem lokalem Speicher.
- Durch das Entfernen von Abhängigkeiten von anderen Diensten können Sie die Dienstverfügbarkeit verbessern. Wenn Sie den Zustand mit Hochverfügbarkeit im Cluster verwalten, sind Sie vor anderen Dienstausfällen oder Latenzproblemen geschützt.

## <a name="how-to-work-with-reliable-services"></a>Arbeiten mit Reliable Services
Service Fabric Reliable Services ermöglichen Ihnen das einfache Erstellen von zustandslosen und zustandsbehafteten Diensten. Weitere Informationen finden Sie unter [Einführung in Reliable Services](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-introduction).
- Berücksichtigen Sie immer das [Abbruchtoken](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-lifecycle#stateful-service-primary-swaps) in der `RunAsync()`-Methode für zustandslose und zustandsbehaftete Dienste und die `ChangeRole()`-Methode für zustandsbehaftete Dienste. Andernfalls weiß Service Fabric nicht, ob Ihr Dienst geschlossen werden kann. Beispielsweise kann die Nichtbeachtung des Abbruchtokens zu wesentlich längeren Upgradezeiten für Anwendungen führen.
-   Öffnen und schließen Sie [Kommunikationslistener](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication) rechtzeitig, und berücksichtigen Sie die Abbruchtoken.
-   Kombinieren Sie niemals synchronen Code mit asynchronem Code. Verwenden Sie beispielsweise nicht `.GetAwaiter().GetResult()` in Ihren asynchronen Aufrufen. Arbeiten Sie in der *gesamten* Aufrufliste asynchron.

## <a name="how-to-work-with-reliable-actors"></a>Arbeiten mit Reliable Actors
Service Fabric Reliable Actors ermöglicht Ihnen das einfache Erstellen von zustandsbehafteten virtuellen Akteuren. Weitere Informationen finden Sie unter [Einführung in Reliable Actors](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction).

- Ziehen Sie unbedingt Veröffentlichen/Abonnieren-Messaging zwischen Ihren Akteuren für die Skalierung der Anwendung in Betracht. Tools, die diesen Dienst anbieten, sind z. B. die [Open-Source-Lösung Service Fabric Pub/Sub von SoCreate](https://service-fabric-pub-sub.socreate.it/) und [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).
- Stellen Sie den Akteurzustand so [präzise wie möglich](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices) dar.
- Verwalten Sie den [Lebenszyklus des Akteurs](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#best-practices). Löschen Sie Akteure, wenn Sie nicht vorhaben, sie noch einmal zu verwenden. Das Löschen von nicht benötigten Akteuren ist besonders wichtig, wenn Sie [flüchtige Zustandsanbieter](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-state-management#state-persistence-and-replication) verwenden, da alle Zustände im Arbeitsspeicher gespeichert werden.
- Aufgrund ihrer [turnusbasierten Parallelität](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-introduction#concurrency) werden Akteure am besten als unabhängige Objekte verwendet. Erstellen Sie keine Graphen mit synchronen Methodenaufrufen mit mehreren Akteuren (von denen jeder höchstwahrscheinlich zu einem separaten Netzwerkaufruf wird), und erstellen Sie keine zirkulären Akteuranforderungen. Diese beeinflussen die Leistung und Skalierung erheblich.
- Kombinieren Sie keinen synchronen Code mit asynchronem Code. Verwenden Sie immer asynchronen Code, um Leistungsprobleme zu vermeiden.
- Nehmen Sie keine zeitintensiven Aufrufe in Akteuren vor. Diese blockieren andere Aufrufe des gleichen Akteurs aufgrund der turnusbasierten Parallelität.
- Wenn Sie mit anderen Diensten über [Service Fabric-Remoting](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-services-communication-remoting) kommunizieren und eine `ServiceProxyFactory` erstellen, dann erstellen Sie die Factory auf der Eben des [Actordiensts](https://docs.microsoft.com/azure/service-fabric/service-fabric-reliable-actors-using) und *nicht* auf der des Akteurs.


## <a name="application-diagnostics"></a>Anwendungsdiagnose
Fügen Sie konsequent [Anwendungsprotokollierung](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-generation-app) in Dienstaufrufen hinzu. Dies hilft bei der Diagnose von Szenarien, in denen Dienste sich gegenseitig aufrufen. Bei der Abrufreihenfolge „A ruft B auf – B ruft C auf – C ruft D auf“ könnte der Aufruf z. B. an jeder beliebigen Stelle zu einem Fehler führen. Wenn die Protokollierung nicht umfassend ist, können Fehler nur schwer diagnostiziert werden. Wenn die Dienste aufgrund des Aufrufaufkommens zu viel protokollieren, sollten Sie zumindest Fehler und Warnungen protokollieren.

## <a name="iot-and-messaging-applications"></a>IoT und Messaginganwendungen
Wenn Sie Nachrichten von [Azure IoT Hub](https://docs.microsoft.com/azure/iot-hub/) oder [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/) lesen, verwenden Sie [ServiceFabricProcessor](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/ServiceFabricProcessor). ServiceFabricProcessor ist in Service Fabric Reliable Services integriert und verwaltet den Zustand des Lesens von den Event Hub-Partitionen und pusht neue Nachrichten über die `IEventProcessor::ProcessEventsAsync()`-Methode an Ihre Dienste.


## <a name="design-guidance-on-azure"></a>Entwurfsleitfaden für Azure
* Besuchen Sie das [Azure Architecture Center](https://docs.microsoft.com/azure/architecture/microservices/). Dort finden Sie Anleitungen zum [Erstellen von Microservices in Azure](https://docs.microsoft.com/azure/architecture/microservices/).

* Lesen Sie [Erste Schritte mit Azure für Gaming](https://docs.microsoft.com/gaming/azure/). Dort finden Sie Anleitungen zum [Verwenden von Service Fabric in Gamingdiensten](https://docs.microsoft.com/gaming/azure/reference-architectures/multiplayer-synchronous-sf).
