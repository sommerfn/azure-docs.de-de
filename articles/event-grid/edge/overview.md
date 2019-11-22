---
title: 'Ereignisgesteuerte Architekturen im Edgebereich: Azure Event Grid unter IoT Edge'
description: Verwenden Sie Azure Event Grid als Modul unter IoT Edge, um Ereignisse zwischen Modulen, Edgegeräten und der Cloud weiterzuleiten.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: overview
ms.date: 10/22/2019
ms.author: babanisa
ms.openlocfilehash: e03429ed3df5bd3518d5e5194bd842b9a4f290ba
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/28/2019
ms.locfileid: "72991493"
---
# <a name="what-is-azure-event-grid-on-azure-iot-edge"></a>Was ist Azure Event Grid unter Azure IoT Edge?
Mit Event Grid unter IoT Edge wird die Leistungsstärke und Flexibilität von Azure Event Grid für alle Veröffentlichen/Abonnieren- und ereignisgesteuerten Szenarien im Edgebereich bereitgestellt. Erstellen Sie Themen, veröffentlichen Sie Ereignisse, und abonnieren Sie mehrere Ziele – unabhängig davon, ob es sich um Module auf demselben Gerät, andere Edgegeräte oder Dienste in der Cloud handelt.

Wie auch in der Cloud, wird das Modul vom Typ „Event Grid unter IoT Edge“ für die bedarfsgesteuerte Verarbeitung von Routing, Filterung und zuverlässiger Bereitstellung von Ereignissen verwendet. Filtern Sie Ereignisse, um sicherzustellen, dass nur relevante Ereignisse an die unterschiedlichen Ereignishandler gesendet werden, indem erweiterte Filter vom Typ „Zeichenfolge“, „Numerisch“ und „Boolesch“ verwendet werden. Mit der Wiederholungslogik wird sichergestellt, dass das Ereignis das Zielmodul, das Edgegerät oder den Clouddienst auch dann erreicht, wenn ein Ziel zum Zeitpunkt der Veröffentlichung nicht verfügbar ist. Auf diese Weise können Sie Event Grid unter IoT Edge als leistungsfähigen Mechanismus für die Speicherung und Weiterleitung nutzen.

Event Grid unter IoT Edge unterstützt sowohl CloudEvents v1.0 als auch benutzerdefinierte Ereignisschemas. Hierbei wird die gleiche Semantik für das Veröffentlichen und Abonnieren wie für Event Grid in der Cloud unterstützt, um die einfache Interoperabilität mit Azure und Drittanbietern zu ermöglichen.

Dieser Artikel enthält eine Übersicht über Azure Event Grid unter IoT Edge. Eine Schritt-für-Schritt-Anleitung zur Verwendung dieses Moduls unter Edge finden Sie unter [Lokales Veröffentlichen/Abonnieren von Ereignissen](pub-sub-events-webhook-local.md). 

![Modell „Event Grid unter IoT Edge“ mit Quellen und Handlern](../media/edge-overview/functional-model.png)

In dieser Abbildung sind einige Möglichkeiten zur Verwendung von Event Grid unter IoT Edge dargestellt. Es handelt sich aber nicht um eine umfassende Liste mit unterstützten Funktionen.

## <a name="when-to-use-event-grid-on-iot-edge"></a>Verwendung von Event Grid unter IoT Edge

Event Grid unter IoT Edge ist so konzipiert, dass ein einheitliches, benutzerfreundliches und zuverlässiges Ereignismodell bereitgestellt wird, um ereignisgesteuerte Architekturen zwischen dem Edgebereich und der Cloud zu erstellen.

Event Grid unter IoT Edge verfügt über eine Laufzeitoberfläche, die an die Oberfläche des Azure-Clouddiensts angepasst ist, sodass Sie immer die gleichen Ereignisse und API-Aufrufe nutzen können. Unabhängig davon, ob Sie Veröffentlichen/Abonnieren in der Cloud, im Edgebereich oder dazwischen nutzen, kann Event Grid unter IoT Edge jetzt als Ihre zentrale Lösung fungieren.

Verwenden Sie Event Grid unter IoT Edge, um einfache Workflows zwischen Modulen auszulösen. Erstellen Sie beispielsweise ein Thema, und veröffentlichen Sie dafür über Ihr Speichermodul „per Speicherblob erstellte“ Ereignisse. Sie können für diese Themen jetzt eine oder mehrere Funktionen oder benutzerdefinierte Module abonnieren.

Erweitern Sie Ihre Funktionalität für die Abläufe zwischen Edgegeräten. Erstellen Sie geräteübergreifende Abonnements, wenn Sie Blobmodulereignisse veröffentlichen und die Rechenleistung von mehreren Edgegeräten in der Nähe nutzen möchten.

Stellen Sie abschließend eine Verbindung mit der Cloud her. Falls Ihre Blobmodulereignisse regelmäßig mit der Cloud synchronisiert werden sollen, sollten Sie die höhere Computeleistung in der Cloud nutzen, oder senden Sie die verarbeiteten Daten entsprechend weiter, und erstellen Sie zusätzliche Clouddienstabonnements.

Event Grid unter IoT Edge verfügt über eine flexible und zuverlässige entkoppelte Ereignisarchitektur.

## <a name="event-sources"></a>Ereignisquellen

Auf ähnliche Weise wie in der Cloud ermöglicht Event Grid unter IoT Edge die direkte Integration zwischen Modulen, um ereignisgesteuerte Architekturen zu erstellen. Derzeit können die Ereignisse von den folgenden Orten aus an Event Grid unter IoT Edge gesendet werden:

* Azure Blob Storage auf IoT Edge
* CloudEvents-Quellen
* Benutzerdefinierte Module und Container per HTTP POST

## <a name="event-handlers"></a>Ereignishandler

Event Grid unter IoT Edge ist so konzipiert, dass Ereignisse an beliebige Orte gesendet werden können. Derzeit werden die folgenden Ziele unterstützt:

* Andere Module, z. B. IoT Hub, Funktionen und benutzerdefinierte Module
* Andere Edgegeräte
* In der Cloud gehostete Dienste, z. B. Azure Event Grid und Azure Functions
* Webhooks

## <a name="supported-environments"></a>Unterstützte Umgebungen
Derzeit werden Umgebungen vom Typ Windows 64 Bit, Linux 64 Bit und ARM 32 Bit unterstützt.

## <a name="concepts"></a>Konzepte

Es gibt fünf Konzepte in Azure Event Grid, die Sie als Einstieg nutzen können:

* **Ereignisse**: Was ist passiert?
* **Ereignisquellen**: Wo hat das Ereignis stattgefunden?
* **Themen**: Der Endpunkt, an den Herausgeber Ereignisse senden.
* **Ereignisabonnements**: Der Endpunkt oder integrierte Mechanismus zum Weiterleiten von Ereignissen, ggf. auch an mehrere Handler. Abonnements werden auch vom Handler verwendet, um eingehende Ereignisse intelligent zu filtern.
* **Ereignishandler**: Die App oder der Dienst, die bzw. der auf das Ereignis reagiert.

## <a name="cost"></a>Kosten

Event Grid unter IoT Edge ist während der öffentlichen Vorschauphase kostenlos.

## <a name="issues"></a>Probleme
Melden Sie alle Probleme, die bei der Verwendung von Event Grid unter IoT Edge auftreten, unter [https://github.com/Azure/event-grid-iot-edge/issues](https://github.com/Azure/event-grid-iot-edge/issues).

## <a name="next-steps"></a>Nächste Schritte

* [Lokales Veröffentlichen und Abonnieren von Ereignissen](pub-sub-events-webhook-local.md)
* [Veröffentlichen und Abonnieren von Ereignissen in der Cloud](pub-sub-events-webhook-cloud.md)
* [Weiterleiten von Ereignissen an die Event Grid-Cloud](forward-events-event-grid-cloud.md)
* [Weiterleiten von Ereignissen an IoT Hub](forward-events-iothub.md)
* [Lokales Reagieren auf Blob Storage-Ereignisse](react-blob-storage-events-locally.md)