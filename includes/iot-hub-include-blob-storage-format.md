---
title: include file
description: include file
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/15/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 27a254b664e2035cc66109f7a473a3e9ef4d730e
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73612070"
---
<!-- This is the note explaining about the avro and json formats when routing to blob storage. -->
> [!NOTE]
> Die Daten können entweder im [Apache Avro](https://avro.apache.org/)-Format (Standardeinstellung) oder im JSON-Format (Vorschauversion) in den Blobspeicher geschrieben werden. 
>    
> Die Funktion zum Codieren im JSON-Format befindet sich in allen Regionen, in denen IoT Hub verfügbar ist, in der Vorschauphase (mit Ausnahme von „USA, Osten“ und „Europa, Westen“). Das Codierungsformat kann nur beim Konfigurieren des Blob Storage-Endpunkt festgelegt werden. Für bereits eingerichtete Endpunkte kann das Format nicht mehr geändert werden. Bei Verwendung der JSON-Codierung müssen Sie in den Nachrichtensystemeigenschaften den Inhaltstyp (contentType) auf „JSON“ und die Inhaltscodierung (contentEncoding) auf „UTF-8“ festlegen. 
>
> Ausführlichere Informationen zur Verwendung eines Blob Storage-Endpunkts finden Sie unter [Leitfaden zum Weiterleiten an den Speicher](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#azure-storage).
>