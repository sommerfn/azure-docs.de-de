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
ms.openlocfilehash: 6f8b1bdd680d43cb14707338d2e37e41114126b0
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70094746"
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
|id|Zeichenfolge|Die ID des Pools.|