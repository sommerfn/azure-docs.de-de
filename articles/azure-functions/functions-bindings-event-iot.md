---
title: Azure IoT Hub-Bindungen für Azure Functions
description: Erfahren Sie, wie IoT Hub-Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.service: azure-functions
ms.topic: reference
ms.date: 03/05/2019
ms.author: cshoe
ms.openlocfilehash: d8b749a294da379e99e61072ff7a3415c508d2ac
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70097646"
---
# <a name="azure-iot-hub-bindings-for-azure-functions"></a>Azure IoT Hub-Bindungen für Azure Functions

Dieser Artikel erläutert das Arbeiten mit Azure Functions-Bindungen für IoT Hub. Die IoT Hub-Unterstützung basiert auf der [Azure Event Hubs-Bindung](functions-bindings-event-hubs.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Pakete: Functions 1.x

Für Azure Functions Version 1.x werden die IoT Hub-Bindungen im NuGet-Paket [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus), Version 2.x bereitgestellt. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs).

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Pakete: Functions 2.x

Verwenden Sie für Functions 2.x das Paket [Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs), Version 3.x. Den Quellcode für das Paket finden Sie im GitHub-Repository [azure-webjobs-sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs).

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

> [!IMPORTANT]
> Die folgenden Codebeispiele verwenden zwar die Event Hub-API, aber die Syntax gilt auch für IoT Hub-Funktionen.

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md)
