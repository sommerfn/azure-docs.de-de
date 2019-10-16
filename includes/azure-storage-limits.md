---
title: include file
description: include file
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/19/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a48c1352e4628d8e1776a9479aceac7c294a2ea1
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/07/2019
ms.locfileid: "72023065"
---
In der folgenden Tabelle werden die für Azure GPv1, GPv2 und Blob-Speicherkonten geltenden Standardgrenzwerte beschrieben. Das Limit für den *Eingang* bezieht sich auf alle Daten aus Anforderungen, die an ein Speicherkonto gesendet werden. Das Limit für den *Ausgang* bezieht sich auf alle Daten aus Anforderungen, die von einem Speicherkonto empfangen werden.

| Resource | Standardlimit |
| --- | --- |
| Anzahl von Speicherkonten pro Region und Abonnement, einschließlich Standard- und Premium-Konten. | 250 |
| Maximale Speicherkontokapazität | 2 PiB für USA und Europa, und 500 TiB für alle anderen Regionen (einschließlich Vereinigtes Königreich)<sup>1</sup>|
| Maximale Anzahl an Blobcontainern, Blobs, Dateifreigaben, Tabellen, Warteschlangen, Entitäten oder Meldungen pro Speicherkonto | Keine Begrenzung |
| Maximale Anforderungsrate<sup>1</sup> pro Speicherkonto | 20.000 Anforderungen pro Sekunde |
| Maximaler Eingang<sup>1</sup> pro Speicherkonto (Regionen US, Europa) | 25GBit/s |
| Maximaler Eingang<sup>1</sup> pro Speicherkonto (Regionen außerhalb von USA und Europa) | 5 GBit/s bei aktiviertem RA-GRS/GRS, 10 GBit/s für LRS/ZRS<sup>2</sup> |
| Maximaler Ausgang für universelle v2-Speicherkonten und Blobspeicherkonten (alle Regionen) | 50 GBit/s |
| Maximaler Ausgang für universelle v1-Speicherkonten (US-Regionen) | 20 GBit/s bei aktiviertem RA-GRS/GRS, 30 GBit/s für LRS/ZRS<sup>2</sup> |
| Maximaler Ausgang für universelle v1-Speicherkonten (US-fremde Regionen) | 10 GBit/s bei aktiviertem RA-GRS/GRS, 15 GBit/s für LRS/ZRS<sup>2</sup> |

<sup>1</sup> Azure Storage Standard-Konten unterstützen höhere Grenzwerte für Kapazität und Eingang auf Anforderung. Wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/support/faq/), um eine Erhöhung der Kontogrenzwerte für Eingang anzufordern. Weitere Informationen finden Sie unter [Ankündigung größerer Speicherkonten mit höherer Skalierung](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/).

<sup>2</sup> Wenn Lesezugriff (RA-GRS/RA-GZRS) aktiviert ist, sind die Ausgangsziele für den sekundären Standort mit denen für den primären Standort identisch. Für die [Azure Storage-Replikation](https://docs.microsoft.com/azure/storage/common/storage-redundancy) sind folgende Optionen verfügbar:  
[!INCLUDE [azure-storage-redundancy](azure-storage-redundancy.md)]

> [!NOTE]
> Für die meisten Szenarien wird die Verwendung von Speicherkonten vom Typ „Allgemein v2“ empfohlen. Sie können ganz einfach ein Upgrade von einem Konto vom Typ „Allgemein v1“ oder einem Blob Storage-Konto auf ein Konto vom Typ „Allgemein v2“ durchführen. Dabei treten keine Ausfallzeiten auf, und Sie müssen keine Daten kopieren.
>
> Weitere Informationen zu Azure Storage-Konten finden Sie unter [Übersicht über Azure Storage-Konten](../articles/storage/common/storage-account-overview.md).

Wenn die Anforderungen Ihrer Anwendung die Skalierbarkeitsziele eines einzelnen Speicherkontos überschreiten, können Sie die Anwendung so erstellen, dass mehrere Speicherkonten verwendet werden. Sie können Ihre Datenobjekte dann basierend auf diesen Speicherkonten partitionieren. Informationen zu Volumenpreisen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Alle Speicherkonten werden in einer flachen Netzwerktopologie ausgeführt und unterstützen die in diesem Artikel beschriebenen Skalierbarkeits- und Leistungsziele, unabhängig vom Zeitpunkt ihrer Erstellung. Weitere Informationen zur flachen Netzwerkarchitektur von Azure Storage sowie zur Skalierbarkeit finden Sie unter [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx) (Hochverfügbarer Cloud-Speicherdienst mit starker Konsistenz).

