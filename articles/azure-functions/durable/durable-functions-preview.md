---
title: Durable Functions-Previewfunktionen – Azure Functions
description: Hier erhalten Sie Informationen Sie zu den Previewfunktionen für Durable Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: article
ms.date: 09/04/2019
ms.author: azfuncdf
ms.openlocfilehash: 8f2560141eac4e7d9fed267d347990e65bfe9c26
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2019
ms.locfileid: "70933247"
---
# <a name="durable-functions-20-preview-azure-functions"></a>Durable Functions 2.0 Preview (Azure Functions)

*Durable Functions* ist eine Erweiterung von [Azure Functions](../functions-overview.md) und [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md), mit der Sie zustandsbehaftete Funktionen in einer serverlosen Umgebung schreiben können. Die Erweiterung verwaltet Status, Prüfpunkte und Neustarts für Sie. Wenn Sie nicht bereits mit Durable Functions vertraut sind, lesen Sie die [Übersichtsdokumentation](durable-functions-overview.md).

Durable Functions 1.x ist ein GA-Feature (allgemeine Verfügbarkeit) von Azure Functions, enthält aber auch mehrere Unterfeatures, die derzeit als öffentliche Vorschauversion verfügbar sind. In diesem Artikel werden neu veröffentlichte Previewfunktionen beschrieben sowie Details dazu, wie sie funktionieren und wie Sie mit ihrer Verwendung beginnen können.

> [!NOTE]
> Diese Previewfunktionen sind Teil eines Durable Functions 2.0-Releases, bei dem es sich derzeit um ein **Vorschauqualitätsrelease** mit mehreren Breaking Changes handelt. Die Azure Functions Durable-Erweiterungspaketbuilds finden Sie auf „nuget.org“ mit Versionsangaben in der Form **2.0.0-betaX**. Diese Builds sind nicht für Produktionsworkloads gedacht, und Folgereleases könnten zusätzliche Breaking Changes enthalten.

## <a name="breaking-changes"></a>Wichtige Änderungen

In Durable Functions 2.0 werden mehrere Breaking Changes eingeführt. Bei vorhandenen Anwendungen ist davon auszugehen, dass sie nicht ohne Codeänderungen mit Durable Functions 2.0 kompatible sein werden. In diesem Abschnitt werden einige der Änderungen aufgeführt:

### <a name="hostjson-schema"></a>host.json-Schema

Der folgende Codeausschnitt zeigt das neue Schema für host.json. Die wichtigsten Änderungen, die Sie beachten sollten, sind die neuen Unterabschnitte:

* `"storageProvider"` (und der Unterabschnitt `"azureStorage"`) für die speicherspezifische Konfiguration
* `"tracking"` für die Konfiguration von Nachverfolgung und Protokollierung
* `"notifications"` (und der Unterabschnitt `"eventGrid"`) für die Konfiguration von Ereignisrasterbenachrichtigungen

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "azureStorage": {
          "connectionStringName": <string>,
          "controlQueueBatchSize": <int?>,
          "partitionCount": <int?>,
          "controlQueueVisibilityTimeout": <hh:mm:ss?>,
          "workItemQueueVisibilityTimeout": <hh:mm:ss?>,
          "trackingStoreConnectionStringName": <string?>,
          "trackingStoreNamePrefix": <string?>,
          "maxQueuePollingInterval": <hh:mm:ss?>
        }
      },
      "tracking": {
        "traceInputsAndOutputs": <bool?>,
        "traceReplayEvents": <bool?>,
      },
      "notifications": {
        "eventGrid": {
          "topicEndpoint": <string?>,
          "keySettingName": <string?>,
          "publishRetryCount": <string?>,
          "publishRetryInterval": <hh:mm:ss?>,
          "publishRetryHttpStatus": <int[]?>,
          "publishEventTypes": <string[]?>,
        }
      },
      "maxConcurrentActivityFunctions": <int?>,
      "maxConcurrentOrchestratorFunctions": <int?>,
      "extendedSessionsEnabled": <bool?>,
      "extendedSessionIdleTimeoutInSeconds": <int?>,
      "customLifeCycleNotificationHelperType": <string?>
  }
}
```

Mit der zunehmenden Stabilität von Durable Functions 2.0 werden weitere Änderungen am Abschnitt `durableTask` von host.json eingeführt. Weitere Informationen zu diesen Änderungen finden Sie in [diesem GitHub-Problem](https://github.com/Azure/azure-functions-durable-extension/issues/641).

### <a name="public-interface-changes"></a>Änderungen an der öffentlichen Schnittstelle

Die verschiedenen „context“-Objekte, die von Durable Functions unterstützt werden, besaßen abstrakte Basisklassen, die für die Verwendung bei Komponententests gedacht waren. Als Bestandteil von Durable Functions 2.0 wurden diese abstrakten Basisklassen durch Schnittstellen ersetzt. Funktionscode, der die konkreten Typen direkt verwendet, ist davon nicht betroffen.

In der folgenden Tabelle werden die Hauptänderungen dargestellt:

| Alter Typ | Neuer Typ |
|----------|----------|
| DurableOrchestrationClientBase | IDurableOrchestrationClient |
| DurableOrchestrationContextBase | IDurableOrchestrationContext |
| DurableActivityContextBase | IDurableActivityContext |
| OrchestrationClientAttribute | DurableClientAttribute |

In dem Fall, wo eine abstrakte Basisklasse virtuelle Methoden enthielt, wurden diese virtuellen Methoden durch Erweiterungsmethoden ersetzt, die in `DurableContextExtensions` definiert sind.

## <a name="entity-functions"></a>Entitätsfunktionen

Ab Durable Functions v2.0.0-alpha haben wir ein neues Konzept für [Entitätsfunktionen](durable-functions-entities.md) eingeführt.

Entitätsfunktionen definieren Vorgänge zum Lesen und Aktualisieren kleinerer Zustandsteile, bekannt als *dauerhafte Entitäten*. Wie Orchestratorfunktionen besitzen Entitätsfunktionen einen speziellen Triggertyp, den *Entitätstrigger*. Im Gegensatz zu Orchestratorfunktionen müssen Entitätsfunktionen keine spezifischen Codeeinschränkungen besitzen. Entitätsfunktionen verwalten Zustände auch explizit, statt Zustände implizit durch die Ablaufsteuerung darzustellen.

Basierend auf dem ursprünglichen Benutzerfeedback haben wir später Unterstützung für ein klassenbasiertes Programmiermodell für Entitäten in Durable Functions v2.0.0-beta1 hinzugefügt.

Weitere Informationen finden Sie im Artikel zu den [Entitätsfunktionen](durable-functions-entities.md).

## <a name="durable-http"></a>Durable HTTP

Ab Durable Functions v2.0.0-beta2 haben wir eine neue Funktion [Durable HTTP](durable-functions-http-features.md) eingeführt, die Folgendes ermöglicht:

* Direktes Aufrufen von HTTP-APIs aus Orchestrierungsfunktionen (mit einigen dokumentierten Einschränkungen)
* Implementieren automatischer clientseitiger HTTP 202-Statusabfragen
* Integrierte Unterstützung für [verwaltete Azure-Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

Weitere Informationen finden Sie im Artikel zu den [HTTP-Funktionen](durable-functions-http-features.md#consuming-http-apis).

## <a name="alternate-storage-providers"></a>Alternativer Speicheranbieter

Das Durable Task Framework unterstützt heutzutage mehrere Speicheranbieter, einschließlich [Azure Storage](https://github.com/Azure/durabletask/tree/master/src/DurableTask.AzureStorage), [Azure Service Bus](https://github.com/Azure/durabletask/tree/master/src/DurableTask.ServiceBus), [In-Memory-Emulator](https://github.com/Azure/durabletask/tree/master/src/DurableTask.Emulator) sowie einen experimentellen [Redis](https://github.com/Azure/durabletask/tree/redis/src/DurableTask.Redis)-Anbieter. Bisher hat die Durable Task-Erweiterung für Azure Functions jedoch nur den Azure Storage-Anbieter unterstützt. Ab Durable Functions 2.0 wird Unterstützung für alternative Speicheranbieter hinzugefügt, beginnend mit dem Redis-Anbieter.

> [!NOTE]
> Durable Functions 2.0 unterstützt nur .NET Standard 2.0-kompatible Anbieter.

### <a name="emulator"></a>Emulator

Der [DurableTask.Emulator](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Emulator/)-Anbieter ist ein Anbieter im lokalen Speicher für nicht dauerhaften Speicher, geeignet für lokale Testszenarios. Er kann mithilfe des folgenden, minimalen **host.json**-Schemas konfiguriert werden:

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "emulator": { "enabled": true }
      }
    }
  }
}
```

### <a name="redis-experimental"></a>Redis (experimentell)

Der [DurableTask.Redis](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Redis/)-Anbieter legt den gesamten Orchestrierungszustand permanent in einem konfigurierten Redis-Cluster ab.

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "redis": {
          "connectionStringName": <string>,
        }
      }
    }
  }
}
```

Der `connectionStringName` muss auf den Namen einer App-Einstellung oder einer Umgebungsvariablen verweisen. Diese App-Einstellung oder Umgebungsvariable sollte einen Redis-Verbindungszeichenfolgenwert in Form von enthalten *Server:Port* enthalten. Z. B. `localhost:6379` zum Herstellen einer Verbindung mit einem lokalen Redis-Cluster.

> [!NOTE]
> Der Redis-Anbieter ist derzeit experimentell und unterstützt nur Funktions-Apps, die auf einem einzelnen Knoten ausgeführt wird. Es ist nicht garantiert, dass der Redis-Anbieter jemals allgemein verfügbar wird, und er kann in einer zukünftigen Version auch entfernt werden.
