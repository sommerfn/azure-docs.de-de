---
title: Azure Functions – Leitfaden zur Fehlerbehandlung | Microsoft-Dokumentation
description: Enthält einen allgemeinen Leitfaden für die Behandlung von Fehlern, die bei der Ausführung Ihrer Funktionen auftreten, und bietet Links zu bindungsspezifischen Fehlerthemen.
services: functions
cloud: ''
documentationcenter: ''
author: craigshoemaker
manager: gwallace
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 02/01/2018
ms.author: cshoe
ms.openlocfilehash: ef8f2d5a63f7924097362f6aa0ebc78cc0f6455f
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480713"
---
# <a name="azure-functions-error-handling"></a>Azure Functions – Fehlerbehandlung

Dieses Thema enthält allgemeine Richtlinien für die Behandlung von Fehlern, die auftreten, wenn Sie Ihre Funktionen ausführen. Es enthält auch Links zu Themen, die bindungsspezifische Fehler beschreiben, die auftreten können. 

## <a name="handing-errors-in-functions"></a>Behandlung von Fehlern in Funktionen
[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

 
## <a name="binding-error-codes"></a>Bindungsfehlercodes

Bei der Integration in Azure-Dienste können Fehler auftreten, deren Ursache in den APIs der zugrunde liegenden Dienste liegt. Links zur Fehlercodedokumentation für diese Dienste finden Sie möglicherweise im Abschnitt **Ausnahmen und Rückgabecodes** der folgenden Trigger- und Bindungsreferenzthemen:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob Storage](functions-bindings-storage-blob.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Queue Storage](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Tabellenspeicherung](functions-bindings-storage-table.md#exceptions-and-return-codes)
