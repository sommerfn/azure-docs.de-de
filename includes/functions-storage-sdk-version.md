---
title: include file
description: include file
services: functions
author: ggailey777
manager: cfowler
ms.service: azure-functions
ms.topic: include
ms.date: 05/17/2018
ms.author: tdykstra
ms.custom: include file
ms.openlocfilehash: c2fff707dcaafac69efcad3dbf33446a7b797396
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608204"
---
### <a name="azure-storage-sdk-version-in-functions-1x"></a>Azure Storage SDK-Version in Functions 1.x

In Functions 1.x verwenden die Storage-Trigger und -Bindungen Version 7.2.1 des Azure Storage SDK (NuGet-Paket [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1)). Wenn Sie auf eine andere Version des Storage SDK verweisen und in Ihrer Funktionssignatur an einen Storage SDK-Typ binden, meldet die Functions-Runtime möglicherweise, dass Bindungen an diesen Typ nicht möglich sind. Die Lösung besteht darin, sicherzustellen, dass Ihr Projekt auf [WindowsAzure.Storage 7.2.1](https://www.nuget.org/packages/WindowsAzure.Storage/7.2.1) verweist.
