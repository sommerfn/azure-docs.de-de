---
title: Datenredundanz in Azure Storage | Microsoft-Dokumentation
description: Die Daten in Ihrem Microsoft Azure Storage-Konto werden stets repliziert, um Beständigkeit und Hochverfügbarkeit sicherzustellen. Die Redundanzoptionen umfassen den lokal redundanten Speicher (LRS), den zonenredundanten Speicher (ZRS), den georedundanten Speicher (GRS), den georedundanten Speicher mit Lesezugriff (RA-GRS), den geozonenredundanten Speicher (GZRS)/Vorschau und den geozonenredundanten Speicher mit Lesezugriff (RA-GZRS).
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/17/2019
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 6a598ad3f68c613a745fd56f3b368e309afeb8b6
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2019
ms.locfileid: "71122974"
---
# <a name="azure-storage-redundancy"></a>Azure Storage-Redundanz

Die Daten in Ihrem Microsoft Azure-Speicherkonto werden stets repliziert, um Beständigkeit und Hochverfügbarkeit sicherzustellen. Azure Storage kopiert Ihre Daten so, dass sie vor geplanten und ungeplanten Ereignissen geschützt sind – von vorübergehend auftretenden Hardwarefehlern und Netzwerk- oder Stromausfällen bis zu schweren Naturkatastrophen usw. Sie können Ihre Daten wahlweise im selben Rechenzentrum, Rechenzentren in derselben Region und sogar übergreifend über geografisch getrennte Regionen replizieren.

Mit der Replikation wird sichergestellt, dass Ihr Speicherkonto auch bei Ausfällen die [Servicelevelvereinbarung (SLA) für Storage](https://azure.microsoft.com/support/legal/sla/storage/) erfüllt. Die Servicelevelvereinbarung enthält Informationen zu Azure Storage-Garantien in Bezug auf Dauerhaftigkeit und Verfügbarkeit.

Azure Storage überprüft regelmäßig die Integrität der gespeicherten Daten mithilfe von Cyclic Redundancy Checks (CRCs). Wenn eine Datenbeschädigung erkannt wird, wird sie unter Verwendung von redundanten Daten repariert. Azure Storage berechnet auch die Prüfsummen für den gesamten Netzwerkdatenverkehr, um eine Beschädigung von Datenpaketen beim Speichern oder Abrufen von Daten zu erkennen.

## <a name="choosing-a-redundancy-option"></a>Wahl einer Redundanzoption

Wenn Sie ein Speicherkonto erstellen, können Sie eine der folgenden Redundanzoptionen auswählen:

[!INCLUDE [azure-storage-redundancy](../../../includes/azure-storage-redundancy.md)]

Die folgende Tabelle zeigt in einer kurzen Übersicht, in welchem Umfang die jeweilige Replikationsstrategie bei einem bestimmten Ereignistyp (oder einem Ereignis ähnlicher Auswirkung) Dauerhaftigkeit und Verfügbarkeit bietet.

| Szenario                                                                                                 | LRS                             | ZRS                              | GRS/RA-GRS                                  | GZRS/RA-GZRS (Vorschauversion)                              |
| :------------------------------------------------------------------------------------------------------- | :------------------------------ | :------------------------------- | :----------------------------------- | :----------------------------------- |
| Nichtverfügbarkeit von Knoten innerhalb eines Rechenzentrums                                                                 | Ja                             | Ja                              | Ja                                  | Ja                                  |
| Ein gesamtes Rechenzentrum (zonal oder nicht zonal) ist nicht mehr verfügbar.                                           | Nein                              | Ja                              | Ja                                  | Ja                                  |
| Ein regionsweiter Ausfall                                                                                     | Nein                              | Nein                               | Ja                                  | Ja                                  |
| Lesezugriff auf Ihre Daten (in einer georeplizierten Remoteregion) bei regionsweiter Nichtverfügbarkeit | Nein                              | Nein                               | Ja (mit RA-GRS)                                   | Ja (mit RA-GZRS)                                 |
| Konzipiert zum Bereitstellen von \_\_ Dauerhaftigkeit von Objekten über ein bestimmtes Jahr                                          | mindestens 99,999999999 % (11 mal die 9) | mindestens 99,9999999999 % (12 mal die 9) | mindestens 99,99999999999999 % (16 mal die 9) | mindestens 99,99999999999999 % (16 mal die 9) |
| Unterstützte Speicherkontotypen                                                                   | GPv2, GPv1, Blob                | GPv2                             | GPv2, GPv1, Blob                     | GPv2                     |
| Verfügbarkeits-SLA für Leseanforderungen | mindestens 99,9 % (99 % bei der kalten Zugriffsebene) | mindestens 99,9 % (99 % bei der kalten Zugriffsebene) | mindestens 99,9 % (99 % bei der kalten Zugriffsebene) für GRS<br /><br />mindestens 99,99 % (99,9 % bei der kalten Zugriffsebene) für RA-GRS | mindestens 99,9 % (99 % bei der kalten Zugriffsebene) für GZRS<br /><br />mindestens 99,99 % (99,9 % bei der kalten Zugriffsebene) für RA-GZRS |
| Verfügbarkeits-SLA für Schreibanforderungen | mindestens 99,9 % (99 % bei der kalten Zugriffsebene) | mindestens 99,9 % (99 % bei der kalten Zugriffsebene) | mindestens 99,9 % (99 % bei der kalten Zugriffsebene) | mindestens 99,9 % (99 % bei der kalten Zugriffsebene) |

Alle Daten in Ihrem Speicherkonto werden repliziert, einschließlich der Blockblobs, Anfügeblobs, Seitenblobs, Warteschlangen, Tabellen und Dateien. Alle Arten von Speicherkonten werden repliziert, obwohl der ZRS ein universelles v2 Storage-Konto erfordert.

Die Preisen für die verschiedenen Redundanzoptionen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/). 

Informationen zu Azure Storage-Garantien in Bezug auf Dauerhaftigkeit und Verfügbarkeit finden Sie im [SLA für Azure Storage](https://azure.microsoft.com/support/legal/sla/storage/).

> [!NOTE]
> Azure Storage Premium unterstützt derzeit nur lokal redundanten Speicher (Locally Redundant Storage, LRS).

## <a name="changing-replication-strategy"></a>Ändern der Replikationsstrategie

Sie können die Replikationsstrategie für Ihr Speicherkonto mithilfe von [Azure-Portal](https://portal.azure.com/), [Azure PowerShell](storage-powershell-guide-full.md), [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) oder einer der [Azure Storage-Clientbibliotheken](https://docs.microsoft.com/azure/index#pivot=sdkstools) ändern. Bei einer Änderung des Replikationstyps Ihres Speicherkontos treten keine Ausfallzeiten auf.

> [!NOTE]
> Derzeit können Sie das-Azure-Portal oder die Azure Storage-Clientbibliotheken nicht verwenden, um Ihr Konto in ZRS, GZRS oder RA-GZRS zu konvertieren. Weitere Informationen zum Migrieren Ihres Kontos zu ZRS finden Sie unter [Zonenredundanter Speicher (ZRS) zum Erstellen hochverfügbarer Azure Storage-Anwendungen](storage-redundancy-zrs.md). Weitere Informationen zum Migrieren von GZRS oder RA-GZRS finden Sie unter [Erstellen von hochverfügbaren Azure Storage-Anwendungen mit zonenredundantem Speicher (GZRS): Vorschau](storage-redundancy-zrs.md).

### <a name="are-there-any-costs-to-changing-my-accounts-replication-strategy"></a>Entstehen mir Kosten für die Änderung der Replikationsstrategie meines Kontos?

Es hängt von Ihrem Konvertierungspfad ab. Die Reihenfolge der Azure Storage-Redundanzangebote vom kostengünstigsten bis zum kostenintensivsten: LRS, ZRS, GRS, RA-GRS, GZRS und RA-GZRS. Beispielsweise fallen beim Wechsel *von* LRS zu einem beliebigen anderen Replikationstyp zusätzliche Kosten an, da Sie in eine komplexere Redundanzebene wechseln. Beim Wechsel *zu* GRS oder RA-GRS fallen Kosten für eine ausgehende Bandbreite an, da Ihre Daten (in Ihrer primären Region) in Ihre sekundäre Remoteregion repliziert werden. Dies sind einmalige Kosten bei der ersten Einrichtung. Nachdem die Daten kopiert sind, fallen keine weiteren Kosten für die Migration an. Ihnen werden nur ggf. die Replikation neuer oder die Aktualisierung vorhandener Daten in Rechnung gestellt. Ausführliche Informationen zu Bandbreitengebühren finden Sie auf der [Seite mit Informationen zu Azure Storage-Preisen](https://azure.microsoft.com/pricing/details/storage/blobs/).

Wenn Sie Ihr Speicherkonto von GRS zu LRS migrieren, entstehen keine zusätzlichen Kosten, aber Ihre replizierten Daten werden am sekundären Standort gelöscht.

Wenn Sie Ihr Speicherkonto von RA-GRS zu GRS oder LRS migrieren, wird das Konto nach dem Umstellungsdatum für weitere 30 Tage als RA-GRS abgerechnet.

## <a name="see-also"></a>Weitere Informationen

- [Lokal redundanter Speicher (LRS): Kostengünstige Datenredundanz für Azure Storage](storage-redundancy-lrs.md)
- [Zonenredundanter Speicher (ZRS): Hochverfügbare Azure Storage-Anwendungen](storage-redundancy-zrs.md)
- [Georedundanter Speicher (GRS): Regionsübergreifende Replikation für Azure Storage](storage-redundancy-grs.md)
- [Erstellen von hochverfügbaren Azure Storage-Anwendungen mit geozonenredundantem Speicher (GZRS): Vorschau](storage-redundancy-gzrs.md).
- [Skalierbarkeits- und Leistungsziele für Azure-Speicher](storage-scalability-targets.md)
- [Entwerfen hochverfügbarer Anwendungen mithilfe von RA-GRS-Speicher](../storage-designing-ha-apps-with-ragrs.md)
- [Microsoft Azure Storage Redundancy Options and Read Access Geo Redundant Storage](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx) (Redundanzoptionen für Microsoft Azure Storage und Lesezugriff auf georedundanten Speicher)
- [SOSP-Bericht: Azure Storage: A highly available cloud storage service with strong consistency](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx) (Hochverfügbarer Cloudspeicherdienst mit starker Konsistenz)
