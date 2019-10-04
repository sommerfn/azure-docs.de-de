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
ms.date: 08/07/2019
ms.author: b-juche
ms.openlocfilehash: 7cf382f511d2ba8452d77bf207f36b749cb31e94
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848787"
---
# <a name="metrics-for-azure-netapp-files"></a>Metriken für Azure NetApp Files

Azure NetApp Files verfügt über Metriken für Folgendes: zugeteilter Speicher, tatsächliche Speichernutzung, Volumedurchsatz, IOPS und Latenz. Durch die Analyse dieser Metriken können Sie ein besseres Verständnis des Nutzungsmusters und der Volumeleistung Ihrer NetApp-Konten entwickeln.  

## <a name="capacity_pools"></a>Nutzungsmetriken für Kapazitätspools

<!-- 
- *Volume pool allocated size*  
    The size (GiB) of the provisioned capacity pool  
--> 
- *Verwendeter zugeordneter Volumepool*  
    Die Gesamtsumme des Volumekontingents (GiB) eines bestimmten Kapazitätspools (Gesamtsumme der bereitgestellten Größen der Volumes im Kapazitätspool)  
    Diese Größe haben Sie beim Erstellen des Volumes ausgewählt.  
- *Gesamte logische Größe des Volumepools*  
    Die Gesamtsumme des logischen Speichers (GiB), der in einem Kapazitätspool volumeübergreifend genutzt wird  
<!-- 
- *Volume pool total snapshot size*  
    The total of incremental logical space used by the snapshots  
-->

## <a name="volumes"></a>Nutzungsmetriken für Volumes

<!-- 
- *Volume allocated size*   
    The volume size (quota) provisioned in GiB  
--> 
- *Logische Größe des Volumes*   
    Der gesamte logische Speicher (GiB), der auf einem Volume genutzt wird  
    In diesem Größenwert ist der logische Speicherplatz enthalten, der von aktiven Dateisystemen und Momentaufnahmen verwendet wird.  
- *Größe der Volumemomentaufnahme*   
   Der inkrementelle logische Speicherplatz, der von Momentaufnahmen auf einem Volume belegt wird  

## <a name="performance-metrics-for-volumes"></a>Leistungsmetriken für Volumes

- *AverageReadLatency*   
    Die durchschnittliche Dauer von Lesevorgängen vom Volume in Millisekunden
- *AverageWriteLatency*   
    Die durchschnittliche Dauer von Lesevorgängen auf dem Volume in Millisekunden
- *ReadIops*   
    Die Anzahl der Lesevorgänge zum Volume pro Sekunde
- *WriteIops*   
    Die Anzahl der Schreibvorgänge auf dem Volume pro Sekunde

## <a name="next-steps"></a>Nächste Schritte

* [Grundlegendes zur Speicherhierarchie von Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Einrichten eines Kapazitätspools](azure-netapp-files-set-up-capacity-pool.md)
* [Erstellen eines Volumes für Azure NetApp Files](azure-netapp-files-create-volumes.md)
