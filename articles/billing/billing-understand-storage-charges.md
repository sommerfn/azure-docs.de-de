---
title: Grundlegendes zur Anwendung des Rabatts für Reservierungen auf Azure Storage | Microsoft-Dokumentation
description: Hier erfahren Sie, wie der Rabatt für reservierte Azure Storage-Kapazität auf Blockblob- und Azure Data Lake Storage Gen2-Ressourcen angewendet wird.
author: tamram
ms.service: billing
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: tamram
ms.openlocfilehash: ebef727cfa291744b3c97299da2a1340f34f5d72
ms.sourcegitcommit: 827248fa609243839aac3ff01ff40200c8c46966
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/07/2019
ms.locfileid: "73746267"
---
# <a name="understand-how-the-reservation-discount-is-applied-to-azure-storage"></a>Grundlegendes zur Anwendung des Rabatts für Reservierungen auf Azure Storage

Nach dem Erwerb von reservierter Azure Storage-Kapazität wird der Reservierungsrabatt automatisch auf Blockblob- und Azure Data Lake Storage Gen2-Ressourcen angewendet, die den Reservierungsbedingungen entsprechen. Der Reservierungsrabatt gilt nur für Speicherkapazität. Bandbreite und Anforderungsrate werden zu den Sätzen für die nutzungsbasierte Bezahlung abgerechnet.

Weitere Informationen zu reservierter Azure Storage-Kapazität finden Sie unter [Optimieren der Kosten für Blobspeicher mit reservierter Kapazität](../storage/blobs/storage-blob-reserved-capacity.md).

Informationen zu den Preisen für Azure Storage-Reservierungen finden Sie in der [Preisübersicht für Blockblobs](https://azure.microsoft.com/pricing/details/storage/blobs/) und in der [Preisübersicht für Azure Data Lake Storage Gen2](https://azure.microsoft.com/pricing/details/storage/data-lake/).

## <a name="how-the-reservation-discount-is-applied"></a>Anwendung des Reservierungsrabatts

Der Rabatt für reservierte Azure Storage-Kapazität wird auf Stundenbasis auf Blockblob- und Azure Data Lake Storage Gen2-Ressourcen angewendet.

Nicht in Anspruch genommener Rabatt für reservierte Azure Storage-Kapazität geht verloren. Wenn Sie also für eine Stunde über keine Blockblob- oder Azure Data Lake Storage Gen2-Ressourcen verfügen, die den Reservierungsbedingungen entsprechen, verlieren Sie eine Reservierungsmenge für diese Stunde. Sie können ungenutzte reservierte Stunden nicht übertragen.

Wenn Sie eine Ressource löschen, gilt der Reservierungsrabatt automatisch für eine andere entsprechende Ressource im angegebenen Reservierungsumfang. Werden im angegebenen Reservierungsumfang keine entsprechenden Ressourcen gefunden, gehen die reservierten Stunden verloren.

## <a name="discount-examples"></a>Beispiele für die Anwendung des Rabatts

Die folgenden Beispiele zeigen, wie der Rabatt für die reservierte Azure Storage-Kapazität abhängig von den jeweiligen Bereitstellungen angewendet wird.

Angenommen, Sie haben für eine Laufzeit von einem Jahr eine reservierte Kapazität von 100 TB in der Region „USA, Westen 2“ erworben. Ihre Reservierung gilt für lokal redundanten Speicher (LRS) der heißen Zugriffsebene.

Die Kosten dieser Beispielreservierung liegen bei 18 540 USD. Sie können entweder den gesamten Betrag vorab bezahlen oder sich für die Zahlung von 12 festen Monatsraten in Höhe von jeweils 1.545 USD entscheiden.

Für diese Beispiele gehen wir davon aus, dass Sie sich für einen monatlichen Zahlungsplan entschieden haben. In den folgenden Szenarien erfahren Sie, was passiert, wenn Sie Ihre reservierte Kapazität unter- oder überschreiten.

### <a name="underusing-your-capacity"></a>Unterschreitung Ihrer Kapazität

Angenommen, Sie haben in einer Stunde innerhalb des Reservierungszeitraums nur 80 TB Ihrer reservierten Kapazität (100 TB) beansprucht. Die verbleibenden 20 TB werden für diese Stunde nicht angewendet und nicht übertragen.

### <a name="overusing-your-capacity"></a>Überschreitung Ihrer Kapazität

Angenommen, Sie haben in einer Stunde innerhalb des Reservierungszeitraums eine Speicherkapazität von 101 TB beansprucht. Der Reservierungsrabatt gilt für 100 TB Ihrer Daten, und die verbleibende Menge von 1 TB wird für diese Stunde zu den Sätzen für die nutzungsbasierte Bezahlung abgerechnet. Wenn Ihre Nutzung in der nächsten Stunde 100 TB beträgt, ist die gesamte Nutzung durch die Reservierung abgedeckt.

## <a name="need-help-contact-us"></a>Sie brauchen Hilfe? Kontakt

Wenn Sie weitere Fragen haben oder Hilfe benötigen, [erstellen Sie eine Supportanfrage](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Nächste Schritte

- [Optimieren der Kosten für Blobspeicher mit reservierter Kapazität](../storage/blobs/storage-blob-reserved-capacity.md)
- [Was sind Azure-Reservierungen?](billing-save-compute-costs-reservations.md)
