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
ms.openlocfilehash: 9326e6dac88fa23a7bb2bc489064aca5ef52980b
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/14/2019
ms.locfileid: "69015938"
---
Zonenredundanter Speicher (ZRS) repliziert Ihre Daten synchron über drei Speichercluster in einer Region. Jeder Speichercluster ist physisch unabhängig von den anderen und befindet sich in einer eigenen Verfügbarkeitszone. Jede Verfügbarkeitszone – und der darin enthaltene ZRS-Cluster – ist autonom und enthält separate Hilfsprogramme und Netzwerkfeatures. Eine Schreibanforderung an ein ZRS-Speicherkonto wird erst dann erfolgreich zurückgegeben, nachdem die Daten in alle Replikate in allen drei Clustern geschrieben wurden.

Wenn Sie Ihre Daten in einem Speicherkonto mit der ZRS-Replikation speichern, können Sie weiter auf Ihre Daten zugreifen und diese verwalten, falls eine Verfügbarkeitszone nicht mehr verfügbar ist. ZRS bietet eine herausragende Leistung bei geringer Latenz. ZRS ermöglicht die gleichen [Skalierbarkeitsziele](../articles/storage/common/storage-scalability-targets.md) wie [lokal redundanter Speicher (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Erwägen Sie die Verwendung von ZRS für Szenarien, für die Konsistenz, Dauerhaftigkeit und Hochverfügbarkeit erforderlich sind. Auch wenn eine Verfügbarkeitszone aufgrund eines Ausfalls oder einer Naturkatastrophe nicht mehr verfügbar ist, ist bei ZRS für das jeweilige Jahr in Bezug auf Speicherobjekte eine Dauerhaftigkeit von mindestens 99,9999999999% (12 Neunen) sichergestellt.

Bei geozonenredundantem Speicher (GZRS) (Vorschau) werden Ihre Daten synchron über drei Azure-Verfügbarkeitszonen in der primären Region repliziert und anschließend asynchron in der sekundären Region repliziert. GZRS bietet Hochverfügbarkeit und maximale Dauerhaftigkeit. GZRS ist darauf ausgelegt, für Objekte eine Dauerhaftigkeit von mindestens 99,99999999999999 Prozent (16 Neunen) in einem bestimmten Jahr bereitzustellen. Aktivieren Sie für den Lesezugriff auf Daten in der sekundären Region den geozonenredundanten Speicher mit Lesezugriff (RA-GZRS). Weitere Informationen zu GZRS finden Sie unter [Geozonenredundante Speicher für Hochverfügbarkeit und maximale Dauerhaftigkeit (Vorschau)](../articles/storage/common/storage-redundancy-lrs.md).

Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Übersicht über Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview).
