---
title: include file
description: include file
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 08/02/2019
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 557e34e8a6bddd36a92b0e212bda3609baa14407
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/20/2019
ms.locfileid: "69641882"
---
Sie können die folgenden Parametertypen für das auslösende Blob verwenden:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* Ein als JSON serialisierbares POCO
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> Erfordert die inout-Bindung `direction` in *function.json* oder `FileAccess.ReadWrite` in einer C#-Klassenbibliothek.

Wenn Sie versuchen, eine Bindung an einen der Storage SDK-Typen herzustellen, und eine Fehlermeldung erhalten, stellen Sie sicher, dass ein Verweis auf [die richtige Storage SDK-Version](#azure-storage-sdk-version-in-functions-1x) vorliegt.

Eine Bindung mit `string`, `Byte[]` oder POCO wird nur bei kleinen Blobs empfohlen, da der gesamte Blobinhalt in den Arbeitsspeicher geladen wird. Im Allgemeinen ist es günstiger, die Typen `Stream` oder `CloudBlockBlob` zu verwenden. Weitere Informationen finden Sie unter [Nebenläufigkeit und Arbeitsspeichernutzung](#trigger---concurrency-and-memory-usage) weiter unten in diesem Artikel.