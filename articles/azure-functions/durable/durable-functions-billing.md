---
title: Abrechnung von Durable Functions – Azure Functions
description: Erfahren Sie mehr über die internen Verhaltensweisen von Durable Functions und wie sich diese auf die Abrechnung für Azure Functions auswirken.
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: overview
ms.date: 08/31/2019
ms.author: azfuncdf
ms.openlocfilehash: b79d50adf932bd5c316fbda9d0964eea7c0484ca
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935458"
---
# <a name="durable-functions-billing"></a>Abrechnung von Durable Functions

[Durable Functions](durable-functions-overview.md) wird genau wie Azure Functions in Rechnung gestellt. Weitere Informationen finden Sie unter [Azure Functions – Preise](https://azure.microsoft.com/pricing/details/functions/). Beim Ausführen von Orchestratorfunktionen im [Nutzungsplan](../functions-scale.md#consumption-plan) von Azure Functions sind einige Verhaltensweisen zu beachten, die bei der Abrechnung auftreten. In den folgenden Abschnitten werden diese Verhaltensweisen und ihre Auswirkungen ausführlicher beschrieben.

## <a name="orchestrator-function-replay-billing"></a>Abrechnung für die Wiedergabe von Orchestratorfunktionen

[Orchestratorfunktionen](durable-functions-orchestrations.md) werden unter Umständen während der gesamten Lebensdauer der Orchestrierung mehrmals wiedergegeben. Jede Wiedergabe wird von der Azure Functions-Runtime als eigenständiger Funktionsaufruf gesehen. Aus diesem Grund wird Ihnen im Azure Functions-Nutzungsplan jede Wiedergabe der Orchestratorfunktion in Rechnung gestellt. Bei anderen Plantypen wird die Wiedergabe von Orchestratorfunktionen nicht berechnet.

## <a name="awaiting-and-yielding-in-orchestrator-functions"></a>Warten und Anhalten in Orchestratorfunktionen

Wenn eine Orchestratorfunktion darauf wartet, dass eine asynchrone Aktion mithilfe von `await` (C#) oder `yield` (JavaScript) abgeschlossen wird, wird die jeweilige Ausführung von der Runtime als abgeschlossen betrachtet. Die Abrechnung für die Orchestratorfunktion wird an diesem Punkt angehalten und erst bei der nächsten Wiedergabe einer Orchestratorfunktion wiederaufgenommen. Die Zeit, in der Sie warten oder die eine Orchestratorfunktion angehalten ist, wird Ihnen nicht in Rechnung gestellt.

> [!NOTE]
> Funktionen, die andere Funktionen aufrufen, werden manchmal als Antimuster betrachtet. Der Grund dafür ist die sogenannte _doppelte Abrechnung_. Wenn eine Funktion eine andere Funktion direkt aufruft, werden beide gleichzeitig ausgeführt. Die aufgerufene Funktion führt aktiv Code aus, während die aufrufende Funktion auf eine Antwort wartet. In dem Fall wird Ihnen die Zeit in Rechnung gestellt, während der die aufrufende Funktion darauf wartet, dass die aufgerufene Funktion ausgeführt wird. Orchestratorfunktionen werden von dieser doppelten Abrechnung nicht beeinträchtigt, da die Abrechnung der Orchestratorfunktion angehalten wird, während sie auf das Ergebnis einer Aktivitätsfunktion (oder einer untergeordneten Orchestrierung) wartet.

## <a name="durable-http-polling"></a>Durable HTTP-Abruf

Orchestratorfunktionen können HTTP-Aufrufe mit langer Ausführungszeit an externe Endpunkte durchführen, wie im Artikel [HTTP-Features](durable-functions-http-features.md) beschrieben. Die `CallHttpAsync`-Methode (C#) und die `callHttp`-Methode (JavaScript) können einen HTTP-Endpunkt intern abfragen, wenn das [asynchrone 202-Muster](durable-functions-http-features.md#http-202-handling) befolgt wird. Für interne HTTP-Abrufvorgänge gibt es derzeit keine direkte Abrechnung. Interne Abrufe führen jedoch unter Umständen zu einer periodischen Wiedergabe der Orchestratorfunktion, und für diese internen Funktionswiedergaben werden Ihnen Standardgebühren in Rechnung gestellt.

## <a name="azure-storage-transactions"></a>Azure-Speichertransaktionen

Durable Functions verwendet standardmäßig Azure Storage, um den Zustand beizubehalten, Nachrichten zu verarbeiten und Partitionen über Blob-Leases zu verwalten. Dieses Speicherkonto befindet sich in Ihrem Besitz, sodass alle Transaktionskosten in Ihrem Azure-Abonnement abgerechnet werden. Weitere Informationen zu den von Durable Functions verwendeten Azure Storage Artefakten finden Sie im Artikel [Aufgabenhubs](durable-functions-task-hubs.md).

Mehrere Faktoren tragen zu den tatsächlichen Azure Storage-Kosten bei, die durch Ihre Durable Functions-Anwendung entstehen.

* Eine einzelne Funktions-App ist einem einzelnen Aufgabenhub zugeordnet, der einen Azure Storage-Ressourcensatz nutzt. Diese Ressourcen werden von allen Durable Functions in einer Funktions-App gemeinsam verwendet. Die tatsächliche Anzahl von Funktionen in der Funktions-App wirkt sich nicht auf Azure Storage-Transaktionskosten aus.
* Jede Funktions-App-Instanz ruft mithilfe eines Abrufalgorithmus für exponentielles Backoff intern mehrere Warteschlangen im Speicherkonto ab. Eine im Leerlauf befindliche Anwendungsinstanz fragt die Warteschlangen seltener ab als eine aktive Anwendung, weshalb die Transaktionskosten niedriger sind. Weitere Informationen zum Verhalten von Durable Functions beim Abrufen von Warteschlangen finden Sie im Abschnitt zum Abrufen von Warteschlangen im Artikel [Leistung und Skalierbarkeit](durable-functions-perf-and-scale.md#queue-polling).
* Wenn die Ausführung im Nutzungs- oder Premium-Plan von Azure Functions erfolgt, ruft der Azure Functions-Skalierungscontroller in regelmäßigen Abständen alle Aufgabenhubwarteschlangen im Hintergrund ab. Bei einer leichten bis mittleren Skalierung werden diese Warteschlangen nur von einer einzelnen Skalierungscontroller-Instanz abgefragt. Wenn die Funktions-App auf eine große Anzahl von Instanzen erweitert wird, können mehr Skalierungscontroller-Instanzen hinzugefügt werden. Diese zusätzlichen Skalierungscontroller-Instanzen können die Gesamtkosten für die Warteschlangentransaktion erhöhen.
* Jede Funktions-App-Instanz konkurriert um einen Satz von Blob-Leases. Diese Instanzen führen in regelmäßigen Abständen Aufrufe an den Azure-Blob-Dienst aus, um gehaltene Leases zu erneuern oder neue Leases abzurufen. Die Anzahl der Blob-Leases hängt von der konfigurierten Partitionsanzahl des Aufgabenhubs ab. Das horizontale Hochskalieren auf eine größere Anzahl von Funktions-App-Instanzen erhöht wahrscheinlich die Azure Storage-Transaktionskosten für diese Leasevorgänge.

Weitere Informationen zu den Preisen für Azure Storage finden Sie in der [Übersicht über die Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Erfahren Sie mehr über die Preise für Azure Functions](https://azure.microsoft.com/pricing/details/functions/)
