---
title: Aktivieren von Blobdaten aus der Archivzugriffsebene
description: Aktivieren Sie Ihre Blobs aus der Archivzugriffsebene, sodass Sie auf die Daten zugreifen können.
services: storage
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/07/2019
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.reviewer: hux
ms.openlocfilehash: 2e7d56a1461dfd89a7309288aadb0ba245d0f885
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/12/2019
ms.locfileid: "68957507"
---
# <a name="rehydrate-blob-data-from-the-archive-tier"></a>Aktivieren von Blobdaten aus der Archivzugriffsebene

Während ein Blob sich auf der Archivzugriffsebene befindet, wird es als offline betrachtet und kann nicht gelesen oder geändert werden. Die Blobmetadaten bleiben online und verfügbar, sodass Sie das Blob und seine Eigenschaften auflisten können. Das Lesen und Ändern von Blobdaten ist nur auf Onlineebenen wie der heißen oder der kalten Zugriffsebene möglich. Es gibt zwei Optionen, um Daten aus der Archivzugriffsebene abzurufen und auf sie zuzugreifen.

1. [Aktivieren eines archivierten Blobs auf einer Onlineebene](#rehydrate-an-archived-blob-to-an-online-tier): Aktivieren Sie ein archiviertes Blob auf der heißen oder kalten Zugriffsebene, indem Sie die Ebene mithilfe des Vorgangs [Blobebene festlegen](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) ändern.
2. [Kopieren eines archivierten Blobs auf eine Onlineebene](#copy-an-archived-blob-to-an-online-tier): Erstellen Sie eine neue Kopie eines archivierten Blobs mithilfe des Vorgangs [Blob kopieren](https://docs.microsoft.com/rest/api/storageservices/copy-blob). Geben Sie einen anderen Blobnamen und als Zielebene eine heiße oder kalte Zugriffsebene an.

 Weitere Informationen zu Ebenen finden Sie unter [Azure Blob Storage: Archivzugriffsebenen „Heiß“, „Kalt“ und „Archiv“](storage-blob-storage-tiers.md).

## <a name="rehydrate-an-archived-blob-to-an-online-tier"></a>Aktivieren eines archivierten Blobs auf einer Onlineebene

[!INCLUDE [storage-blob-rehydration](../../../includes/storage-blob-rehydrate-include.md)]

## <a name="copy-an-archived-blob-to-an-online-tier"></a>Kopieren eines archivierten Blobs auf eine Onlineebene

Wenn Sie ein Blob nicht aktivieren möchten, können Sie den Vorgang [Blob kopieren](https://docs.microsoft.com/rest/api/storageservices/copy-blob) ausführen. Das ursprüngliche Blob bleibt unverändert im Archiv, und Sie arbeiten mit dem neuen Blob auf der heißen oder kalten Zugriffsebene. Bei Verwendung des Kopiervorgangs können Sie die optionale Eigenschaft *x-ms-rehydrate-priority* auf „Standard“ oder „Hoch“ (Vorschau) festlegen.

Archivblobs können nur auf Onlinezielebenen kopiert werden. Das Kopieren eines Archivblobs in ein anderes Archivblob wird nicht unterstützt.

Das Kopieren eines Blobs aus dem Archiv nimmt einige Zeit in Anspruch. Im Hintergrund aktiviert der Vorgang **Blob kopieren** das Archivblob vorübergehend, um ein neues Onlineblob auf der Zielebene zu erstellen. Dieses neue Blob wird erst dann verfügbar, wenn die vorübergehende Aktivierung aus dem Archiv abgeschlossen ist und die Daten in das neue Blob geschrieben wurden.

## <a name="pricing-and-billing"></a>Preise und Abrechnung

Die Vorgänge zum Aktivieren von Blobs aus dem Archiv auf einer heißen oder kalten Zugriffsebene werden als Lese- und Datenabrufvorgänge abgerechnet. Vorgänge mit hoher Priorität (Vorschau) verursachen höhere Vorgangs- und Datenabrufkosten als Vorgänge mit Standardpriorität. Aktivierungsvorgänge mit hoher Priorität werden auf Ihrer Rechnung als separater Posten ausgewiesen. Wenn eine Anforderung zum Abruf eines einige Gigabytes großen Archivblobs mit hoher Priorität über fünf Stunden dauert, werden Ihnen nicht die Gebühren für einen Abruf mit hoher Priorität berechnet. Stattdessen werden die Standardgebühren für Abrufvorgänge berechnet.

Die Vorgänge zum Kopieren von Blobs aus dem Archiv auf eine heiße oder kalte Zugriffsebene werden als Lese- und Datenabrufvorgänge abgerechnet. Für die Erstellung der neuen Kopie wird ein Schreibvorgang in Rechnung gestellt. Gebühren für frühes Löschen fallen beim Kopieren in ein Onlineblob nicht an, weil das Quellblob auf der Archivzugriffsebene unverändert bleibt. Gebühren für Vorgänge mit hoher Priorität dagegen fallen an.

Blobs auf Archivzugriffsebene müssen mindestens 180 Tage lang gespeichert werden. Wenn archivierte Blobs vor Ablauf von 180 Tagen gelöscht oder aktiviert werden, fallen Gebühren für frühes Löschen an.

> [!NOTE]
> Weitere Informationen zu den Preisen für Blockblobs und Datenaktivierung finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/). Weitere Informationen zu den Kosten für ausgehende Datenübertragungen finden Sie unter [Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="next-steps"></a>Nächste Schritte

* [Informationen zu Blobspeicherebenen](storage-blob-storage-tiers.md)
* Prüfen der Preise für „Heiß“, „Kalt“ und „Archiv“ in Blobspeicher- und GPv2-Konten nach Region unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Verwalten des Azure Blob Storage-Lebenszyklus](storage-lifecycle-management-concepts.md)
* [Überprüfen der Preise für Datenübertragungen unter Datenübertragungen – Preisdetails](https://azure.microsoft.com/pricing/details/data-transfers/)
