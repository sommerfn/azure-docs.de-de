---
title: Azure Functions – Anleitung zu Fehlerbehandlung
description: Erfahren Sie, wie Sie Fehler in Azure Functions mit Links zu bestimmten Bindungsfehlern behandeln können.
services: functions
cloud: ''
documentationcenter: ''
author: craigshoemaker
manager: gwallace
ms.assetid: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: cshoe
ms.openlocfilehash: d301940998175adb6469e46cff2de918b5cae9df
ms.sourcegitcommit: 116bc6a75e501b7bba85e750b336f2af4ad29f5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/20/2019
ms.locfileid: "71155287"
---
# <a name="azure-functions-error-handling"></a>Azure Functions – Fehlerbehandlung

Das Behandeln von Fehlern in Azure Functions ist wichtig, um verlorene Daten und verpasste Ereignisse zu vermeiden sowie um die Integrität Ihrer Anwendung zu überwachen.

Dieser Artikel beschreibt allgemeine Strategien für die Fehlerbehandlung zusammen mit Links zu bindungsspezifischen Fehlern.

## <a name="handling-errors"></a>Behandeln von Fehlern

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

## <a name="binding-error-codes"></a>Bindungsfehlercodes

Bei der Integration in Azure-Dienste können Fehler auftreten, deren Ursache in den APIs der zugrunde liegenden Dienste liegt. Informationen zu bindungsspezifischen Fehlern finden Sie im Abschnitt **Ausnahmen und Rückgabecodes** der folgenden Artikel:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob Storage](functions-bindings-storage-blob.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Queue Storage](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Tabellenspeicherung](functions-bindings-storage-table.md#exceptions-and-return-codes)
