---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 5df0fe23183b53f13d6c6545e248724324434cde
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2019
ms.locfileid: "69029789"
---
Verfügbare Replikationsoptionen für ein Speicherkonto:

* [Lokal redundanter Speicher (LRS):](../articles/storage/common/storage-redundancy-lrs.md) Eine einfache, kostengünstige Replikationsstrategie. Daten werden drei Mal synchron in der primären Region repliziert.
* [Zonenredundanter Speicher (ZRS):](../articles/storage/common/storage-redundancy-zrs.md) Replikation für Szenarien, die Hochverfügbarkeit erfordern. Daten werden synchron über drei Azure-Verfügbarkeitszonen hinweg in der primären Region repliziert.
* [Georedundanter Speicher (Geo-Redundant Storage, GRS)](../articles/storage/common/storage-redundancy-grs.md): Regionsübergreifende Replikation zum Schutz vor regionalen Ausfällen. Die Daten werden in der primären Region drei Mal synchron repliziert und dann asynchron in die sekundäre Region repliziert. Aktivieren Sie für den Lesezugriff auf die Daten in der sekundären Region den georedundanten Speicher mit Lesezugriff (RA-GRS).
* [Geozonenredundanter Speicher (GZRS) (Vorschau)](../articles/storage/common/storage-redundancy-gzrs.md): Replikation für Szenarien, die sowohl Hochverfügbarkeit als auch maximale Dauerhaftigkeit erfordern. Die Daten werden synchron über drei Azure-Verfügbarkeitszonen in die primären Region repliziert und anschließend asynchron in die sekundäre Region repliziert. Aktivieren Sie für den Lesezugriff auf die Daten in der sekundären Region den geozonenredundanten Speicher mit Lesezugriff (RA-GZRS).
