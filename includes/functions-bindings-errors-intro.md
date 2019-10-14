---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 178fa7d5f129a12736ec068fca605ba24cd37839
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2019
ms.locfileid: "71955863"
---
In Azure Functions auftretende Fehler können einen der folgenden Ursprünge haben:

- Verwendung integrierter Azure Functions-[Trigger und -Bindungen](..\articles\azure-functions\functions-triggers-bindings.md)
- Aufrufe an APIs zugrunde liegender Azure-Dienste
- Aufrufe an REST-Endpunkte
- Aufrufe an Clientbibliotheken, Pakete oder Drittanbieter-APIs

Die folgenden bewährten Methoden für die Fehlerbehandlung sind wichtig, um den Verlust von Daten oder verpasste Nachrichten zu vermeiden. Empfohlene Vorgehensweisen zur Fehlerbehandlung umfassen folgende Aktionen:

- [Aktivieren von Application Insights](../articles/azure-functions/functions-monitoring.md)
- [Verwenden strukturierter Fehlerbehandlung](#use-structured-error-handling)
- [Design für Idempotenz](../articles/azure-functions/functions-idempotent.md)
- [Implementieren von Wiederholungsrichtlinien](../articles/azure-functions/functions-reliable-event-processing.md) (falls zutreffend)

### <a name="use-structured-error-handling"></a>Verwenden strukturierter Fehlerbehandlung

Das Erfassen und Veröffentlichen von Fehlern ist kritisch für die Überwachung der Integrität Ihrer Anwendung. Die oberste Ebene jedes Funktioncodes sollte einen try/catch-Block enthalten. Im catch-Block können Sie Fehler erfassen und veröffentlichen.

### <a name="retry-support"></a>Wiederholungsunterstützung

Die folgenden Trigger bieten eine integrierte Unterstützung der Wiederholung:

* [Azure Blob Storage](../articles/azure-functions/functions-bindings-storage-blob.md)
* [Azure Queue Storage](../articles/azure-functions/functions-bindings-storage-queue.md)
* [Azure Service Bus (Warteschlange/Thema)](../articles/azure-functions/functions-bindings-service-bus.md)

Standardmäßig wiederholen diese Trigger Anforderungen bis zu fünfmal. Nach dem fünften Wiederholungsversuch schreiben beide Trigger eine Nachricht in die [Warteschlange für nicht verarbeitbare Nachrichten](..\articles\azure-functions\functions-bindings-storage-queue.md#trigger---poison-messages).

Sie müssen Wiederholungsrichtlinien für alle anderen Trigger- oder Bindungstypen manuell implementieren. Manuelle Implementierungen können das Schreiben von Fehlerinformationen in eine [Warteschlange für nicht verarbeitbare Nachrichten](..\articles\azure-functions\functions-bindings-storage-blob.md#trigger---poison-blobs) umfassen. Durch das Schreiben in eine Warteschlange für nicht verarbeitbare Nachrichten erhalten Sie die Möglichkeit, Vorgänge zu einem späteren Zeitpunkt zu wiederholen. Dieser Ansatz ist mit dem vom Blob Storage-Trigger verwendeten identisch.
