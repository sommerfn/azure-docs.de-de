---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a416d1c6e813be558f034e15576c57efa6073788
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2019
ms.locfileid: "70968576"
---
**Ausgehende Datenübertragungen**: [Ausgehende Datenübertragungen](https://azure.microsoft.com/pricing/details/bandwidth/) (Daten, die von den Azure-Datencentern ausgehen) verursachen Kosten bei der Bandbreitenverwendung.

**Transaktionen:** Ihnen wird die Anzahl von Transaktionen berechnet, die Sie für einen verwalteten Standarddatenträger durchführen. Für SSD Standard gilt jeder E/A-Vorgang kleiner oder gleich 256 KiB Durchsatz als ein einzelner E/A-Vorgang. E/A-Vorgänge mit einem Durchsatz größer als 256 KiB gelten als mehrere Ein-bzw. Ausgabevorgänge der Größe 256 KiB. Für HDD Standard wird jeder E/A-Vorgang als einzelne Transaktion betrachtet, und zwar unabhängig von der E/A-Größe.

Ausführliche Informationen zu den Preisen für Managed Disks, einschließlich der Transaktionskosten, finden Sie unter [Verwaltete Datenträger – Preise](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-disk-vm-reservation-fee"></a>Reservierungsgebühr für virtuellen Computer auf Ultra-Datenträger

Virtuelle Azure-Computer können angeben, ob sie mit Ultra-Datenträgern kompatibel sind. Ein mit Ultra-Datenträgern kompatibler virtueller Computer weist dedizierte Bandbreitenkapazität zwischen der Compute-VM-Instanz und der Blockspeicher-Skalierungseinheit zu, um die Leistung zu optimieren und die Wartezeiten zu verringern. Das Hinzufügen dieser Funktion auf dem virtuellen Computer führt dazu, dass die Reservierungsgebühr nur erhoben wird, wenn Sie die Ultra-Datenträgerfunktion auf dem virtuellen Computer aktiviert haben, ohne ihr einen Ultra-Datenträger anzufügen. Wenn ein Ultra-Datenträger an den mit Ultra-Datenträgern kompatiblen virtuellen Computer angefügt ist, fällt diese Gebühr nicht an. Diese Gebühr fällt pro auf der VM bereitgestellter vCPU an. 

> [!Note]
> Für [VM-Größen mit eingeschränkter Anzahl der Kerne](../articles/virtual-machines/linux/constrained-vcpu.md) ergibt sich die Reservierungsgebühr anhand der tatsächlichen Anzahl der vCPUs und nicht anhand der eingeschränkten Kerne. Für Standard_E32-8s_v3 basiert die Reservierungsgebühr auf 32 Kernen. 

Preisdetails für Ultra-Datenträger finden Sie auf der [Seite mit der Preisübersicht für Azure-Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/).
