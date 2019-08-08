---
title: Metriken für Azure NetApp Files | Microsoft-Dokumentation
description: Enthält eine Beschreibung der Metriken für Azure NetApp Files.
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
ms.date: 08/06/2019
ms.author: b-juche
ms.openlocfilehash: 9a56fb27fdf9e196291942041d68b249d7f16648
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/06/2019
ms.locfileid: "68839234"
---
# <a name="metrics-for-azure-netapp-files"></a>Metriken für Azure NetApp Files

Azure NetApp Files verfügt über Metriken für Folgendes: zugeteilter Speicher, tatsächliche Speichernutzung, Volumedurchsatz, IOPS und Latenz. Durch die Analyse dieser Metriken können Sie ein besseres Verständnis des Nutzungsmusters und der Volumeleistung Ihrer NetApp-Konten entwickeln.  

## <a name="capacity_pools"></a>Nutzungsmetriken für Kapazitätspools

<!-- 
- *Volume pool allocated size*  
    This is the size (GiB) of the provisioned capacity pool.  
--> 
- *Verwendeter zugeordneter Volumepool*  
    Dies ist die Gesamtsumme des Volumekontingents (GiB) eines bestimmten Kapazitätspools (Gesamtsumme der bereitgestellten Größen der Volumes im Kapazitätspool). Diese Größe haben Sie beim Erstellen des Volumes ausgewählt.  
- *Gesamte logische Größe des Volumepools*  
    Dies ist die Gesamtsumme des logischen Speichers (GiB), der in einem Kapazitätspool volumeübergreifend genutzt wird.  
<!-- 
- *Volume pool total snapshot size*  
    This is the total of incremental logical space used by the snapshots.  
-->

## <a name="volumes"></a>Nutzungsmetriken für Volumes

<!-- 
- *Volume allocated size*   
    This is the volume size (quota) provisioned in GiB.  
--> 
- *Logische Größe des Volumes*   
    Dies ist der gesamte logische Speicher (GiB), der auf einem Volume genutzt wird. In diesem Größenwert ist der logische Speicherplatz enthalten, der von aktiven Dateisystemen und Momentaufnahmen verwendet wird.  
- *Größe der Volumemomentaufnahme*   
    Dies ist der inkrementelle logische Speicherplatz, der von Momentaufnahmen auf einem Volume verwendet wird.  

## <a name="next-steps"></a>Nächste Schritte

* [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)
* [Erstellen eines Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
