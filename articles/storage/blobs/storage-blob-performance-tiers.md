---
title: Leistungsstufen von Azure-Blockblobspeicher – Azure Storage
description: Leistungsstufen für Azure-Blobspeicher.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 10/02/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: clausjor
ms.openlocfilehash: ac483a338b7d71142b89b13e41fc048346ac037f
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803919"
---
# <a name="azure-block-blob-storage-performance-tiers"></a>Leistungsstufen von Azure-Blockblobspeicher

Da Unternehmen leistungsrelevante cloudbasierte Anwendungen bereitstellen, ist es wichtig, über Optionen für eine kostengünstige Datenspeicherung auf verschiedenen Leistungsstufen zu verfügen.

Azure-Blockblobspeicher bietet zwei verschiedene Leistungsstufen:

- Premium: Optimiert für hohe Transaktionsraten und einstellige konsistente Speicherlatenzen
- Standard: Optimiert für hohe Kapazität und hohen Durchsatz

Die folgenden Überlegungen gelten für die unterschiedlichen Leistungsstufen:

- Standardleistung ist in allen [Azure-Regionen](https://azure.microsoft.com/global-infrastructure/services/?products=storage) verfügbar. Premium-Leistung ist in [ausgewählten Regionen](https://azure.microsoft.com/global-infrastructure/services/?products=storage) verfügbar.
- Die Premium-Leistung bietet optimierte Preise für Anwendungen mit hohen Transaktionsraten, um die [Gesamtspeicherkosten für diese Workloads zu senken](https://azure.microsoft.com/blog/reducing-overall-storage-costs-with-azure-premium-blob-storage/).
- Premium-Leistung erfordert die Verwendung von Blockblob-Speicherkonten, die „Blockblob“ und „Anfügeblob“ unterstützen.
- Die Standardleistung ist mit den Speicherkonten vom Typ „Universell v1“, „Universell v2“ sowie mit Blobspeicherkonten verfügbar.
- Premium- und Standardleistung unterstützen jeweils [Blockblobs mit hohem Durchsatz](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/). Blockblobs mit hohem Durchsatz sind für Premium-Leistung von mehr als 256 KiB verfügbar. Blockblobs mit hohem Durchsatz sind für Standardleistung von mehr als 4 MiB „Put Block“ oder „Put Blob“-Größen verfügbar.
- Premium-Leistung ist derzeit nur mit lokal redundantem Speicher (LRS) verfügbar.

## <a name="premium-performance"></a>Premium-Leistung

Blockblobspeicher mit Premium-Leistung macht Daten über Hochleistungshardware verfügbar. Daten werden auf SSD-Datenträgern (Solid State Drives) gespeichert, die für niedrige Latenz optimiert sind. Der Durchsatz ist bei SSDs im Vergleich zu herkömmlichen Festplatten höher.

Blockblobspeicher mit Premium-Leistung eignet sich ideal für Workloads, die schnelle und konsistente Antwortzeiten benötigen. Er ist am besten für Workloads geeignet, die viele kleine Transaktionen durchführen.

## <a name="standard-performance"></a>Standardleistung

Die Standardleistung unterstützt verschiedene [Zugriffsebenen](storage-blob-storage-tiers.md), um Daten möglichst kostengünstig zu speichern. Sie ist für eine hohe Kapazität und einen hohen Durchsatz bei großen Datasets optimiert.

## <a name="blob-lifecycle-management"></a>Lebenszyklusverwaltung für Blobs

Die Blob Storage-Lebenszyklusverwaltung bietet eine umfassende, regelbasierte Richtlinie:

- Premium: Daten laufen am Ende ihres Lebenszyklus ab.
- Standard: Daten werden auf die bestmögliche Zugriffsebene übertragen und am Ende des Lebenszyklus laufen die Daten ab.

Weitere Informationen finden Sie unter [Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md).

Daten, die in einem Premium-Blockblob-Speicherkonto gespeichert sind, können nicht zwischen den Ebenen „Heiß“, „Kalt“ und „Archiv“ verschoben werden. Sie können jedoch Blobs von einem Blockblob-Speicherkonto auf die Zugriffsebene „Heiß“ in einem *anderen* Konto kopieren. Verwenden Sie die API [Put Block From URL](/rest/api/storageservices/put-block-from-url) oder [AzCopy v10](../common/storage-use-azcopy-v10.md), um Daten in ein anderes Konto zu kopieren. Die API **Put Block From URL** kopiert Daten synchron auf dem Server. Der Aufruf wird erst abgeschlossen, wenn alle Daten vom ursprünglichen Serverstandort zum Zielstandort verschoben wurden.

## <a name="next-steps"></a>Nächste Schritte

Evaluieren von „Heiß“, „Kalt“ und „Archiv“ in GPv2- und Blobspeicherkonten

- Prüfen der Verfügbarkeit von „Hot“, „Cool“ und „Archiv“ nach Region unter [Azure-Regionen](https://azure.microsoft.com/regions/#services)
- [Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md)
- [Informationen zum Aktivieren von Blobdaten aus der Archivzugriffsebene](storage-blob-rehydration.md)
- [Auswerten der Nutzung vorhandener Speicherkonten durch Aktivierung von Azure Storage-Metriken](../common/storage-enable-and-view-metrics.md)
- Prüfen der Preise für „Heiß“, „Kalt“ und „Archiv“ in Blobspeicher- und GPv2-Konten nach Region unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
- [Überprüfen der Preise für Datenübertragungen unter Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/)
