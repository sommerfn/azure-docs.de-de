---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/04/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d25efa8c666fa91c7bd652a7cf931ea6ca379aa2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133306"
---
Zonenredundanter Speicher (ZRS) repliziert Ihre Daten synchron über drei Speichercluster in einer Region. Jeder Speichercluster ist physisch unabhängig von den anderen und befindet sich in einer eigenen Verfügbarkeitszone. Jede Verfügbarkeitszone – und der darin enthaltene ZRS-Cluster – ist autonom und enthält separate Hilfsprogramme und Netzwerkfeatures. Eine Schreibanforderung an ein ZRS-Speicherkonto wird erst dann erfolgreich zurückgegeben, nachdem die Daten in alle Replikate in allen drei Clustern geschrieben wurden.

Wenn Sie Ihre Daten in einem Speicherkonto mit der ZRS-Replikation speichern, können Sie weiter auf Ihre Daten zugreifen und diese verwalten, falls eine Verfügbarkeitszone nicht mehr verfügbar ist. ZRS bietet eine herausragende Leistung bei geringer Latenz. ZRS ermöglicht die gleichen [Skalierbarkeitsziele](../articles/storage/common/storage-scalability-targets.md) wie [lokal redundanter Speicher (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Erwägen Sie die Verwendung von ZRS für Szenarien, für die Konsistenz, Dauerhaftigkeit und Hochverfügbarkeit erforderlich sind. Auch wenn eine Verfügbarkeitszone aufgrund eines Ausfalls oder einer Naturkatastrophe nicht mehr verfügbar ist, ist bei ZRS für das jeweilige Jahr in Bezug auf Speicherobjekte eine Dauerhaftigkeit von mindestens 99,9999999999% (12 Neunen) sichergestellt.

Weitere Informationen zu Verfügbarkeitszonen finden Sie unter [Übersicht über Verfügbarkeitszonen](https://docs.microsoft.com/azure/availability-zones/az-overview).