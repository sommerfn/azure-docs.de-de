---
title: Anpassen der Größe des Kapazitätspools oder eines Volumes für Azure NetApp Files | Microsoft-Dokumentation
description: In diesem Artikel wird beschrieben, wie die Größe eines Kapazitätspools oder eines Volumes geändert wird.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/03/2018
ms.author: b-juche
ms.openlocfilehash: f08eaee038ad18c600826dea6fe0fd85935de59a
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2019
ms.locfileid: "57761972"
---
# <a name="resize-a-capacity-pool-or-a-volume"></a>Ändern der Größe eines Kapazitätspools oder Volumes
Sie können die Größe eines Kapazitätspools oder eines Volumes nach Bedarf anpassen. 

## <a name="resize-the-capacity-pool"></a>Anpassen der Größe des Kapazitätspools 

Sie können die Größe des Kapazitätspools in 4-TiB-Schritten erhöhen oder verringern. Durch eine Größenänderung des Kapazitätspools wird die erworbene Azure NetApp Files-Kapazität geändert.

1. Klicken Sie auf dem Blatt „Manage NetApp Account“ (NetApp-Konto verwalten) auf den Kapazitätspool, dessen Größe Sie anpassen möchten. 
2. Klicken Sie mit der rechten Maustaste auf den Namen des Kapazitätspools, oder klicken Sie am Ende der Zeile mit dem Kapazitätspool auf das Symbol „...“, um das Kontextmenü anzuzeigen. 
3. Mithilfe der Kontextmenüoptionen können Sie die Größe des Kapazitätspools ändern oder ihn löschen.

## <a name="resize-a-volume"></a>Ändern der Größe eines Volumes

Sie können die Größe eines Volumes nach Bedarf anpassen. Der Kapazitätsverbrauch eines Volumes wird mit der bereitgestellten Kapazität des dazugehörigen Pools verrechnet.

1. Klicken Sie auf dem Blatt „Manage NetApp Account“ auf **Volumes**. 
2. Klicken Sie mit der rechten Maustaste auf den Namen des Volumes, dessen Größe Sie anpassen möchten, oder klicken Sie auf am Ende der Zeile mit dem Volume auf das Symbol „...“, um das Kontextmenü anzuzeigen.
3. Mithilfe der Kontextmenüoptionen können Sie die Größe des Volumes ändern oder es löschen.

