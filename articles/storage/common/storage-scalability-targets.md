---
title: Skalierbarkeits- und Leistungsziele für Azure Storage – Speicherkonten
description: Informationen zu den Skalierbarkeits- und Leistungszielen, einschließlich Kapazität, Anforderungsrate sowie eingehende und ausgehende Bandbreite für Azure-Speicherkonten.
services: storage
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 293d0de024ad815b4755d48833b7058c29ebcd5a
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513293"
---
# <a name="azure-storage-scalability-and-performance-targets-for-storage-accounts"></a>Skalierbarkeits- und Leistungsziele für Speicherkonten in Azure Storage

In diesem Artikel werden die Skalierbarkeits- und Leistungsziele für Azure-Speicherkonten erläutert. Die hier aufgelisteten Ziele für Skalierbarkeit und Leistung sind hochgesteckte Ziele, die jedoch erreichbar sind. In jedem Fall hängen die von Ihrem Speicherkonto erzielte Anforderungsrate und Bandbreite von der Größe der gespeicherten Objekte, den verwendeten Zugriffsmustern und der Art der von der Anwendung ausgeführten Workload ab.

Testen Sie unbedingt Ihren Dienst, um festzustellen, ob seine Leistung Ihren Anforderungen entspricht. Wenn möglich, vermeiden Sie plötzliche Lastspitzen bei der Datenverkehrsrate, und stellen Sie sicher, dass der Datenverkehr über alle Partitionen verteilt ist.

Wenn Ihre Anwendung die Grenze dessen erreicht, was eine Partition an Workload bewältigen kann, dann gibt Azure Storage den Fehlercode 503 (Server ausgelastet) oder den Fehlercode 500 (Zeitüberschreitung für Vorgang) zurück. Wenn 503-Fehler auftreten, sollte die Anwendung so geändert werden, dass sie eine exponentielle Backoffrichtlinie für Wiederholungen verwendet. Durch exponentielle Backoffs kann die Auslastung der Partition verringert werden, um die Datenverkehrsspitzen bei dieser Partition auszugleichen.

## <a name="storage-account-scale-limits"></a>Skalierungslimits für Speicherkonten

[!INCLUDE [azure-storage-limits](../../../includes/azure-storage-limits.md)]

## <a name="premium-performance-storage-account-scale-limits"></a>Skalierungslimits für Speicherkonten der Leistungsstufe „Premium“

[!INCLUDE [azure-premium-limits](../../../includes/azure-storage-limits-premium.md)]

## <a name="storage-resource-provider-scale-limits"></a>Skalierungslimits für Speicherressourcenanbieter

[!INCLUDE [azure-storage-limits-azure-resource-manager](../../../includes/azure-storage-limits-azure-resource-manager.md)]

## <a name="azure-blob-storage-scale-targets"></a>Skalierbarkeitsziele für Azure Blob Storage

[!INCLUDE [storage-blob-scale-targets](../../../includes/storage-blob-scale-targets.md)]

## <a name="azure-files-scale-targets"></a>Skalierbarkeitsziele für Azure Files

Weitere Informationen zu den Skalierbarkeits- und Leistungszielen für Azure Files und die Azure-Dateisynchronisierung finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Files](../files/storage-files-scale-targets.md).

> [!IMPORTANT]
> Die Einschränkungen für Speicherkonten gelten für alle Freigaben. Zentrales Hochskalieren bis zum Maximalwert für Speicherkonten ist nur möglich, wenn es nur eine Freigabe pro Speicherkonto gibt.
>
> Standarddateifreigaben über 5 TiB unterliegen neben regionalen auch bestimmten anderen Einschränkungen.
> Eine Liste der Einschränkungen, regionale Informationen und Anweisungen dazu, wie Sie diese größeren Dateifreigaben aktivieren, finden Sie im Abschnitt [Onboarding für größere Dateifreigaben (Standard-Tarif)](../files/storage-files-planning.md#onboard-to-larger-file-shares-standard-tier) des Planungshandbuchs für Azure Files.

[!INCLUDE [storage-files-scale-targets](../../../includes/storage-files-scale-targets.md)]

### <a name="premium-files-scale-targets"></a>Skalierbarkeitsziele für Premium-Dateien

Es gibt drei Kategorien von Einschränkungen, die für Premium-Dateien zu berücksichtigen sind: Speicherkonten, Freigaben und Dateien.

Beispiel:  Eine einzelne Freigabe kann einen IOPS von 100.000 erreichen, und eine einzelne Datei kann bis zu einem IOPS von 5.000 zentral hochskalieren. Wenn Sie beispielsweise drei Dateien in einer Freigabe haben, entspricht der maximale IOPS aus dieser Freigabe 15.000.

#### <a name="premium-file-share-limits"></a>Freigabelimits für Premiumdateien

[!INCLUDE [storage-files-premium-scale-targets](../../../includes/storage-files-premium-scale-targets.md)]

### <a name="azure-file-sync-scale-targets"></a>Skalierbarkeitsziele für die Azure-Dateisynchronisierung

Azure-Dateisynchronisierung wurde mit dem Ziel der unbegrenzten Nutzung entwickelt, aber unbegrenzte Nutzung ist nicht immer möglich. Die folgende Tabelle gibt an, welche Grenzen für Tests von Microsoft gelten und welche Ziele feste Grenzwerte sind:

[!INCLUDE [storage-sync-files-scale-targets](../../../includes/storage-sync-files-scale-targets.md)]

## <a name="azure-queue-storage-scale-targets"></a>Skalierbarkeitsziele für Azure Queue Storage

[!INCLUDE [storage-queues-scale-targets](../../../includes/storage-queues-scale-targets.md)]

## <a name="azure-table-storage-scale-targets"></a>Skalierbarkeitsziele für Azure-Tabellen

[!INCLUDE [storage-table-scale-targets](../../../includes/storage-tables-scale-targets.md)]

## <a name="see-also"></a>Weitere Informationen

- [Speicher – Preisdetails](https://azure.microsoft.com/pricing/details/storage/)
- [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../../azure-subscription-service-limits.md)
- [Azure Storage-Replikation](../storage-redundancy.md)
- [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](../storage-performance-checklist.md)
