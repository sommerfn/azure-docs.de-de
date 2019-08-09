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
ms.openlocfilehash: bc0ffec1bfe55ee81573feffc58e1adb11421eec
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667446"
---
# <a name="azure-functions-error-handling"></a>Azure Functions – Fehlerbehandlung

Dieses Thema enthält allgemeine Richtlinien für die Behandlung von Fehlern, die auftreten, wenn Sie Ihre Funktionen ausführen. Es enthält auch Links zu Themen, die bindungsspezifische Fehler beschreiben, die auftreten können. 

## <a name="handling-errors-in-functions"></a>Behandeln von Fehlern in Funktionen
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
