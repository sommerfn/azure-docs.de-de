---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 10/24/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 52dbf03c7d4c2c2de565d6793fc867cf5b886db3
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2019
ms.locfileid: "73992203"
---
| SSD Premium-Größen | P1* | P2* | P3* | P4 | P6 | P10 | P15 | P20 | P30 | P40 | P50 | P60 | P70 | P80 |
|-------------------|----|----|----|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| Datenträgergröße in GiB | 4 | 8 | 16 | 32 | 64 | 128 | 256 | 512 | 1024 | 2\.048 | 4\.096 | 8\.192 | 16.384 | 32.767 |
| IOPS pro Datenträger | 120 | 120 | 120 | 120 | 240 | 500 | 1\.100 | 2\.300 | 5\.000 | 7\.500 | 7\.500 | 16.000 | 18.000 | 20.000 |
| Durchsatz pro Datenträger | 25 MiB/s | 25 MiB/s | 25 MiB/s | 25 MiB/s | 50 MiB/s | 100 MiB/s | 125 MiB/s | 150 MiB/s | 200 MiB/s | 250 MiB/s | 250 MiB/s| 500 MiB/s | 750 MiB/s | 900 MiB/s |
| Max. Burst-IOPS pro Datenträger** | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 |
| Max. Burstdurchsatz pro Datenträger** | 170 MiB/s | 170 MiB/s | 170 MiB/s | 170 MiB/s | 170 MiB/s | 170 MiB/s | 170 MiB/s | 170 MiB/s |
| Max. Burstdauer** | 30 Min.  | 30 Min.  | 30 Min.  | 30 Min.  | 30 Min.  | 30 Min.  | 30 Min.  | 30 Min.  |

\*Bezeichnet eine Datenträgergröße, die sich derzeit in der Vorschauphase befindet. Informationen zur regionalen Verfügbarkeit finden Sie unter [Neue Datenträgergrößen: verwaltet und nicht verwaltet](https://docs.microsoft.com/azure/virtual-machines/linux/faq-for-disks#new-disk-sizes-managed-and-unmanaged).

\**Bezeichnet eine Funktion, die sich derzeit in der Vorschauphase befindet. Weitere Informationen finden Sie unter [Regionale Verfügbarkeit](https://docs.microsoft.com/azure/virtual-machines/linux/disk-bursting#regional-availability).
