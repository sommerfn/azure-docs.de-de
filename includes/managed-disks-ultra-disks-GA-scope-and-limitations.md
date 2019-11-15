---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 11/04/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c847b110cb821ebe3ca7e706fcaec592dcc58fa8
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73600977"
---
Derzeit gibt es für Ultra-Datenträger weitere Einschränkungen, die wie folgt lauten:

- Werden in den folgenden Regionen mit jeweils einer unterschiedlichen Anzahl von Verfügbarkeitszonen unterstützt:
    - USA (Ost) 2
    - East US
    - USA, Westen 2
    - Asien, Südosten
    - Nordeuropa
    - Europa, Westen
    - UK, Süden 
- Können nur mit Verfügbarkeitszonen verwendet werden. (Verfügbarkeitsgruppen und einzelne VM-Bereitstellungen außerhalb der Zonen können keine Ultra-Datenträger anfügen.)
- Werden nur auf den folgenden VM-Serien unterstützt:
    - [ESv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - [DSv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - FSv2
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Nicht jede VM-Größe ist in allen unterstützten Regionen mit Ultra-Datenträgern verfügbar.
- Sind nur als Datenträger für Daten verfügbar und unterstützen nur für physische Sektorgröße von 4K.  
- Können nur als leere Datenträger erstellt werden.  
- Unterstützen noch keine Datenträger-Momentaufnahmen, VM-Images, Verfügbarkeitsgruppen oder Azure Disk Encryption
- Unterstützen noch keine Integration in Azure Backup oder Azure Site Recovery.
- Die IOPS-Obergrenze liegt bei allgemein verfügbaren virtuellen Computern derzeit bei 80.000.