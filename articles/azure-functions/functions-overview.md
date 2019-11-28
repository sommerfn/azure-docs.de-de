---
title: Übersicht zu Azure Functions
description: Hier erfahren Sie, wie Sie mithilfe von Azure Functions in wenigen Minuten asynchrone Workloads optimieren.
author: mattchenderson
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.topic: overview
ms.date: 10/03/2017
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 28a2732965ee0a19f69dcd0fc2cd7705cb5f94a1
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230430"
---
# <a name="an-introduction-to-azure-functions"></a>Einführung in Azure Functions  
Azure Functions ist eine Lösung, mit der Sie ganz einfach kleinere Codeelemente (Funktionen) in der Cloud ausführen können. Sie können sich auf das Schreiben des Codes beschränken, der für das jeweilige Problem erforderlich ist, ohne sich über eine gesamte Anwendung oder die Infrastruktur für ihre Ausführung Gedanken machen zu müssen. Mit Functions können Sie noch produktiver entwickeln und Ihre bevorzugte Programmiersprache verwenden, z. B. C#, Java, JavaScript, PowerShell und Python. Bezahlen Sie nur für die Zeit, in der Ihr Code ausgeführt wird, und setzen Sie auf die flexiblen Möglichkeiten zur Skalierung von Azure. Mit Azure Functions können Sie in Microsoft Azure [serverlose](https://azure.microsoft.com/solutions/serverless/) Anwendungen entwickeln.

Dieses Thema bietet einen allgemeinen Überblick über Azure Functions. Unter [Erstellen Sie Ihre erste Funktion in Azure Functions](functions-create-first-azure-function.md)können Sie sich direkt mit der Verwendung von Azure Functions vertraut machen. Technische Informationen zu Functions finden Sie in der [Entwicklerreferenz](functions-reference.md).

## <a name="features"></a>Features
Azure Functions bietet unter anderem folgende zentrale Features:

* **Auswahl der Sprache**: Schreiben Sie Funktionen wahlweise mit C#, Java, JavaScript, Python und anderen Sprachen. Eine vollständige Liste finden Sie unter [Unterstützte Sprachen](supported-languages.md).
* **Preismodell mit nutzungsbasierter Bezahlung** : Bezahlen Sie nur für die Zeit, in der Ihr Code ausgeführt wird. Informationen hierzu finden Sie unter der Hostingoption „Verbrauchstarif“ im [Preisabschnitt](#pricing).  
* **Eigene Abhängigkeiten** : Functions unterstützt NuGet und NPM, sodass Sie Ihre bevorzugten Bibliotheken verwenden können.  
* **Integrierte Sicherheit** : Schützen Sie per HTTP ausgelöste Funktionen mit OAuth-Anbietern wie Azure Active Directory, Facebook, Google, Twitter und Microsoft-Konto.  
* **Vereinfachte Integration** : Profitieren Sie von der einfachen Nutzung von Azure-Diensten und SaaS-Angeboten (Software-as-a-Service). Beispiele finden Sie im Abschnitt [Integrationen](#integrations).  
* **Flexible Entwicklung**: Programmieren Sie Ihre Funktionen direkt im Portal, oder richten Sie Continuous Integration ein, und stellen Sie Ihren Code über [GitHub](../app-service/scripts/cli-continuous-deployment-github.md), [Azure DevOps Services](../app-service/scripts/cli-continuous-deployment-vsts.md) und andere [unterstützte Entwicklungstools](../app-service/deploy-local-git.md) bereit.  
* **Open Source** : Die Laufzeit von Functions ist eine Open-Source-Anwendung und [auf GitHub verfügbar](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Welche Möglichkeiten bestehen mit Functions?
Functions ist eine hervorragende Lösung zum Verarbeiten von Daten, Integrieren von Systemen, Arbeiten mit dem Internet der Dinge (Internet of Things, IoT) und Erstellen einfacher APIs und Microservices. Erwägen Sie die Verwendung von Functions für Aufgaben wie die Bild- oder Auftragsverarbeitung oder die Dateiwartung bzw. für alle Aufgaben, die Sie nach einem Zeitplan durchführen möchten. 

In Functions stehen unter anderem folgende Vorlagen für zentrale Szenarien zur Verfügung:

* **HTTPTrigger** : Dient zum Auslösen der Ausführung Ihres Codes mithilfe einer HTTP-Anforderung. Ein Beispiel finden Sie unter [Erstellen Ihrer ersten Funktion](functions-create-first-azure-function.md).
* **TimerTrigger** : Dient zum Ausführen einer Bereinigung oder anderer Batch-Tasks nach einem vordefinierten Zeitplan. Ein Beispiel finden Sie unter [Erstellen einer Funktion in Azure, die von einem Timer ausgelöst wird](functions-create-scheduled-function.md).
* **CosmosDBTrigger**: Verarbeitet Azure Cosmos-DB-Dokumente, wenn diese hinzugefügt oder in Auflistungen in einer NoSQL-Datenbank aktualisiert werden. Weitere Informationen finden Sie unter [Azure Cosmos DB-Bindungen für Azure Functions 2.x (Vorschauversion)](functions-bindings-cosmosdb-v2.md).
* **BlobTrigger** : Dient zum Verarbeiten von Azure Storage-Blobs beim Hinzufügen zu Containern. Sie können diese Funktion beispielsweise zum Anpassen der Größe von Bildern verwenden. Weitere Informationen finden Sie unter [Azure Functions – Blob Storage-Bindungen](functions-bindings-storage-blob.md).
* **QueueTrigger** : Dient zum Reagieren auf eingehende Meldungen der Azure Storage-Warteschlange. Weitere Informationen finden Sie unter [Azure Queue Storage-Bindungen für Azure Functions](functions-bindings-storage-queue.md).
* **EventGridTrigger**: Reagieren auf Ereignisse, die einem Abonnement in Azure Event Grid übermittelt werden. Unterstützt ein Modell auf Abonnementbasis für den Empfang von Ereignissen, das Filtern einbezieht. Eine gute Lösung für das Erstellen ereignisbasierter Architekturen. Ein Beispiel finden Sie unter [Automatisieren der Größenänderung von hochgeladenen Images per Event Grid](../event-grid/resize-images-on-storage-blob-upload-event.md).
* **EventHubTrigger**: Reagiert auf Ereignisse, die an einen Azure Event Hub übermittelt werden. Dies ist besonders für Anwendungsinstrumentierung, Benutzeroberflächen oder Workflowverarbeitung sowie für IoT-Szenarien (Internet of Things) geeignet. Weitere Informationen finden Sie unter [Event Hubs-Bindungen für Azure Functions](functions-bindings-event-hubs.md).
* **ServiceBusQueueTrigger**: Dient zum Verknüpfen Ihres Codes mit anderen Azure-Diensten oder lokalen Diensten durch Lauschen auf Meldungswarteschlangen. Weitere Informationen finden Sie unter [Service Bus-Bindungen von Azure Functions](functions-bindings-service-bus.md).
* **ServiceBusTopicTrigger**: Dient zum Verknüpfen Ihres Codes mit anderen Azure-Diensten oder lokalen Diensten durch Abonnieren von Themen. Weitere Informationen finden Sie unter [Service Bus-Bindungen von Azure Functions](functions-bindings-service-bus.md).

Azure Functions unterstützt *Trigger* zum Starten der Codeausführung sowie *Bindungen* zur Vereinfachung der Programmierung für Eingabe- und Ausgabedaten. Eine ausführliche Beschreibung der Trigger und Bindungen von Azure Functions finden Sie in der [Entwicklerreferenz zu Triggern und Bindungen in Azure Functions](functions-triggers-bindings.md).

## <a name="integrations"></a>Integrationen
Azure Functions kann in eine Reihe von Azure- und Drittanbieterdiensten integriert werden. Diese können Sie zum Auslösen Ihrer Funktion und zum Starten der Ausführung oder als Ein- und Ausgabe für Ihren Code verwenden. Azure Functions unterstützt folgende Dienstintegrationen:

* Azure Cosmos DB
* Azure Event Hubs
* Azure Event Grid
* Azure Notification Hubs
* Azure Service Bus (Warteschlangen und Themen)
* Azure Storage (Blob, Warteschlangen und Tabellen)
* Lokal (mit Service Bus)
* Twilio (SMS-Nachrichten)

## <a name="pricing"></a>Was kostet Functions?
Azure Functions bietet zwei Arten von Tarifen. Wählen Sie den, der Ihren Anforderungen am besten entspricht: 

* **Verbrauchstarif für**: Wenn Ihre Funktion ausgeführt wird, stellt Azure die erforderlichen Verarbeitungsressourcen bereit. Sie müssen sich nicht um die Ressourcenverwaltung kümmern und bezahlen nur für die Zeit, in der Ihr Code ausgeführt wird. 
* **App Service-Plan**: Funktionen werden auf die gleiche Weise ausgeführt wie Ihre Web-Apps. Wenn Sie App Service bereits für Ihre anderen Anwendungen verwenden, können Sie Ihre Funktionen ohne zusätzliche Kosten unter dem gleichen Tarif ausführen. 

Weitere Informationen zu Hostingplänen finden Sie unter [Vergleich von Hostingplänen für Azure Functions](functions-scale.md). Ausführliche Preisinformationen finden Sie auf der Seite [Functions – Preise](https://azure.microsoft.com/pricing/details/functions/).

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen Sie Ihre erste Funktion in Azure Functions](functions-create-first-azure-function.md)  
  Legen Sie direkt los, und erstellen Sie Ihre erste Funktion mithilfe der Azure Functions-Schnellstartanleitung. 
* [Entwicklerreferenz zu Azure Functions](functions-reference.md)  
  Enthält weitere technische Informationen zur Azure Functions-Laufzeit sowie eine Referenz für das Programmieren von Funktionen sowie für das Festlegen von Triggern und Bindungen.
* [Testen von Azure Functions](functions-test-a-function.md)  
  Beschreibt verschiedene Tools und Techniken zum Testen Ihrer Funktionen
* [How to scale Azure Functions (Skalieren von Azure Functions) (Skalieren von Azure Functions)](functions-scale.md)  
  Beschreibt die für Azure Functions verfügbaren Servicepläne (einschließlich des Hostingplans „Verbrauchstarif“) und enthält Informationen zur Wahl des geeigneten Plans. 
* [Was ist Azure App Service?](../app-service/overview.md)  
  Azure Functions nutzt Azure App Service für Kernfunktionen wie Bereitstellungen, Umgebungsvariablen und Diagnosen. 

