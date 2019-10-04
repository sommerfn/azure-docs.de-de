---
title: 'Azure Batch: Ereignis zum Starten des Löschvorgangs von Pools | Microsoft-Dokumentation'
description: Referenz zum Batch-Ereignis zum Starten des Löschvorgangs von Pools.
services: batch
author: laurenhughes
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: 76f3a15d88166160f28461c34ee03ff7c8880a59
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70258418"
---
# <a name="pool-delete-start-event"></a>Ereignis zum Starten des Löschvorgangs von Pools

 Dieses Ereignis wird ausgegeben, wenn ein Löschvorgang eines Pools gestartet wurde. Da das Löschen des Pools ein asynchrones Ereignis ist, können Sie damit rechnen, dass ein Ereignis zum Abschluss des Löschvorgangs von Pools ausgegeben wird, sobald der Löschvorgang abgeschlossen ist.

 Das folgende Beispiel zeigt den Text eines Ereignisses zum Starten des Löschvorgangs von Pools.

```
{
    "id": "myPool1"
}
```

|Element|Typ|Notizen|
|-------------|----------|-----------|
|`id`|Zeichenfolge|Die ID des Pools.|
