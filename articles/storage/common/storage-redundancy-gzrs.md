---
title: 'Erstellen von hochverfügbaren Azure Storage-Anwendungen mit zonenredundantem Speicher (GZRS): Vorschau | Microsoft-Dokumentation'
description: Mit georedundantem Speicher (GZRS) wird die Hochverfügbarkeit von zonenredundantem Speicher (ZRS) mit Schutz vor regionalen Ausfällen kombiniert, der von georedundantem Speicher (GRS) bereitgestellt wird. Daten in einem GZRS-Speicherkonto werden über Azure-Verfügbarkeitszonen in die primäre Region repliziert sowie auch in eine sekundäre geografische Region zum Schutz vor regionalen Notfällen.
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: e040533acdd8979b7b43358c74d1b729dafa2c66
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/15/2019
ms.locfileid: "74111835"
---
# <a name="build-highly-available-azure-storage-applications-with-geo-zone-redundant-storage-gzrs-preview"></a>Erstellen von hochverfügbaren Azure Storage-Anwendungen mit zonenredundantem Speicher (GZRS): Vorschau

Mit der Vorschau von georedundantem Speicher (GZRS) wird die Hochverfügbarkeit von [zonenredundantem Speicher (ZRS)](storage-redundancy-zrs.md) mit Schutz vor regionalen Ausfällen kombiniert, der von [georedundantem Speicher (GRS)](storage-redundancy-grs.md) bereitgestellt wird. Daten in einem GZRS-Speicherkonto werden über drei [Azure-Verfügbarkeitszonen](../../availability-zones/az-overview.md) in die primäre Region repliziert sowie auch in eine sekundäre geografische Region zum Schutz vor regionalen Notfällen. Jeder Azure-Region ist innerhalb des gleichen geografischen Gebiets eine andere Region als Regionspartner zugeordnet. Weitere Informationen und Ausnahmen finden Sie in der [Dokumentation](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

Mit einem GZRS-Speicherkonto können Sie weiterhin Daten lesen und schreiben, wenn eine Verfügbarkeitszone nicht verfügbar oder nicht wiederherstellbar ist. Außerdem sind Ihre Daten auch bei einem regionalen Komplettausfall oder einem Notfall beständig gespeichert, nach dem die primäre Region nicht mehr wiederherstellbar ist. GZRS ist darauf ausgelegt, für Objekte eine Dauerhaftigkeit von mindestens 99,99999999999999 Prozent (16 Neunen) in einem bestimmten Jahr bereitzustellen. GZRS bietet außerdem die gleichen[Skalierbarkeitsziele](storage-scalability-targets.md) wie LRS, ZRS, GRS oder RA-GRS. Optional können Sie den Lesezugriff auf Daten in der sekundären Region mit georedundantem Speicher mit Lesezugriff (RA-GZRS) aktivieren, wenn Ihre Anwendungen bei einem Notfall in der primären Region Daten lesen sollen.

Microsoft empfiehlt die Verwendung von GZRS für Anwendungen, die Konsistenz, Dauerhaftigkeit, Hochverfügbarkeit, hervorragende Leistung und Resilienz bei der Notfallwiederherstellung erfordern. Aktivieren Sie für zusätzliche Sicherheit des Lesezugriffs auf die sekundäre Region im Falle eines regionalen Notfalls RA-GZRS für Ihr Speicherkonto.

## <a name="about-the-preview"></a>Informationen zur Vorschau

Nur Speicherkonten vom Typ „Allgemein v2“ unterstützen GZRS und RA-GZRS. Weitere Informationen zu Arten von Speicherkontotypen finden Sie unter [Übersicht über Azure Storage-Konten](storage-account-overview.md). GZRS und RA-GZRS unterstützen Blockblobs, Seitenblobs (die keine VHD-Datenträger sind), Dateien, Tabellen und Warteschlangen.

GZRS und RA-GZRS sind derzeit in den folgenden Regionen als Vorschau verfügbar:

- Europa, Norden
- Europa, Westen
- UK, Süden
- USA, Osten
- USA (Ost 2)
- USA, Mitte

Microsoft arbeitet daran, GZRS und RA-GZRS in weiteren Azure-Regionen zu aktivieren. Sehen Sie regelmäßig auf der Seite [Azure-Updates](https://azure.microsoft.com/updates/) nach, um Informationen zu unterstützten Regionen zu erhalten.

Weitere Informationen zu den Preisen für die Vorschauversion finden Sie unter den GZRS-Vorschaupreisen für [Blobs](https://azure.microsoft.com/pricing/details/storage/blobs), [Dateien](https://azure.microsoft.com/pricing/details/storage/files/), [Warteschlangen](https://azure.microsoft.com/pricing/details/storage/queues/) und [Tabellen](https://azure.microsoft.com/pricing/details/storage/tables/).

> [!IMPORTANT]
> Microsoft empfiehlt, Vorschaufunktionen nicht für Produktionsworkloads zu verwenden.

## <a name="how-gzrs-and-ra-gzrs-work"></a>Funktionsweise von GZRS und RA-GZRS

Wenn Daten in ein Speicherkonto mit aktiviertem GZRS oder RA-GZRS geschrieben werden, werden diese Daten zuerst synchron in der primären Region in drei Verfügbarkeitszonen repliziert. Die Daten werden anschließend asynchron in eine zweite Region repliziert, die Hunderte von Kilometern entfernt ist. Wenn die Daten in die sekundäre Region geschrieben werden, werden Sie in dieser Region drei Mal mithilfe von [lokal redundantem Speicher (LRS)](storage-redundancy-lrs.md) weiter synchron repliziert.

> [!IMPORTANT]
> Die asynchrone Replikation beinhaltet eine Verzögerung zwischen dem Zeitpunkt, zu dem diese Daten in der primären Region geschrieben werden, und dem Zeitpunkt, zu dem sie in die sekundäre Region repliziert werden. Bei einem regionalen Notfall gehen Änderungen, die noch nicht in der sekundären Region repliziert wurden, möglicherweise verloren, wenn die Daten nicht in der primären Region wiederhergestellt werden können.

Wenn Sie ein Speicherkonto erstellen, geben Sie an, wie die Daten in diesem Konto repliziert werden sollen. Außerdem geben Sie die primäre Region für dieses Konto an. Das Regionspaar für ein georepliziertes Konto wird basierend auf der primären Region bestimmt und kann nicht geändert werden. Weitere aktuelle Informationen zu unterstützten Azure-Regionen finden Sie unter [Geschäftskontinuität und Notfallwiederherstellung (BCDR): Azure-Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). Weitere Informationen zum Erstellen eines Speicherkontos mit GZRS oder RA-GZRS finden Sie unter [Erstellen eines Speicherkontos](storage-quickstart-create-account.md).

### <a name="use-ra-gzrs-for-high-availability"></a>Verwenden von RA-GZRS für Hochverfügbarkeit

Wenn Sie RA-GZRS für Ihr Speicherkonto aktivieren, können Ihre Daten sowohl vom sekundären Endpunkt als auch vom primären Endpunkt für Ihr Speicherkonto gelesen werden. Der sekundäre Endpunkt fügt das Suffix *–secondary* an den Kontonamen an. Wenn Ihr primärer Endpunkt für den Blob-Dienst z. B. `myaccount.blob.core.windows.net` ist, dann ist Ihr sekundärer Endpunkt `myaccount-secondary.blob.core.windows.net`. Die Zugriffsschlüssel für das Speicherkonto sind für die primären und sekundären Endpunkte identisch.

Um RA-GZRS bei einem regionalen Ausfall nutzen zu können, müssen Sie Ihre Anwendung im Vorfeld dafür konzipieren, dieses Szenario zu verarbeiten. Ihre Anwendung sollte vom primären Endpunkt lesen und in ihn schreiben, aber auf den sekundären Endpunkt umschalten, falls die primäre Region nicht verfügbar sein sollte. Anleitungen dazu, wie Sie mit RA-GZRS Hochverfügbarkeit erreichen können, finden Sie unter [Entwerfen hochverfügbarer Anwendungen mithilfe von RA-GZRS oder RA-GRS](https://docs.microsoft.com/azure/storage/common/storage-designing-ha-apps-with-ragrs).

Da Daten asynchron in die sekundären Region repliziert werden, liegt die sekundäre Region oft hinter der primären Region zurück. Um zu ermitteln, welche Schreibvorgänge in die sekundäre Region repliziert wurden, überprüft Ihre Anwendung den Zeitpunkt der letzten Synchronisierung für Ihr Speicherkonto. Alle Schreibvorgänge, die vor der letzten Synchronissierungszeit in die primäre Region geschrieben wurden, wurden erfolgreich in die sekundäre Region repliziert, sodass sie für das Lesen aus der sekundären Region zur Verfügung stehen. Alle Schreibvorgänge, die nach der letzten Synchronisationszeit in die primäre Region geschrieben wurden, können ggf. in die sekundäre Region repliziert worden sein (oder auch nicht), sodass sie möglicherweise nicht für Lesevorgänge zur Verfügung stehen.

Sie können den Wert der Eigenschaft **Last Sync Time** (Letzte Synchronisierungszeit) mit Azure PowerShell, der Azure CLI oder einer der Azure Storage-Clientbibliotheken abfragen. Die Eigenschaft **Last Sync Time** ist ein GMT-Datums-/Uhrzeitwert.

Zusätzliche Hinweise zur Leistung und Skalierbarkeit mit RA-GZRS finden Sie in der [Checkliste zur Microsoft Azure Storage-Leistung und -Skalierbarkeit](storage-performance-checklist.md).

### <a name="availability-zone-outages"></a>Ausfälle von Verfügbarkeitszonen

Im Falle eines Ausfalls, der eine Verfügbarkeitszone in der primären Region betrifft, können Ihre Anwendungen nahtlos weiter aus dem Speicherkonto lesen und in dieses schreiben, indem sie die anderen Verfügbarkeitszonen für diese Region verwenden. Microsoft empfiehlt, Vorgehensweisen für die Behandlung vorübergehender Fehler bei Verwendung von GZRS oder ZRS anzuwenden. Zu diesen Vorgehensweisen gehört die Implementierung von Wiederholungsrichtlinien mit exponentiellem Backoff.

Wenn eine Verfügbarkeitszone nicht verfügbar ist, führt Azure Netzwerkupdates durch, z.B. durch die Festlegung neuer DNS-Ziele. Diese Updates können sich auf Ihre Anwendung auswirken, wenn Sie auf Daten zugreifen, bevor die Updates abgeschlossen sind.

### <a name="regional-outages"></a>Regionale Ausfälle

Wenn ein Fehler die gesamte primäre Region betrifft, versucht Microsoft zunächst, die primäre Region wiederherzustellen. Wenn eine Wiederherstellung nicht möglich ist, führt Microsoft ein Failover in die sekundäre Region aus, sodass die sekundäre Region zur neuen primären Region wird. Wenn für das Speicherkonto RA-GZRS aktiviert ist, können Anwendungen, die für dieses Szenario entwickelt wurden, aus der sekundären Region lesen, während sie auf das Failover warten. Wenn RA-GZRS für das Speicherkonto nicht aktiviert ist, können Anwendungen erst nach Abschluss des Failovers aus der sekundären Zone lesen.

> [!NOTE]
> GZRS und RA-GZRS sind derzeit nur in der Region „USA, Osten“ als Vorschau verfügbar. Das vom Kunden verwaltete Kontofailover (Vorschau) ist in „USA, Osten 2“ noch nicht verfügbar, sodass Kunden derzeit keine Kontofailoverereignisse mit GZRS- und RA-GZRS-Konten verwalten können. Während der Vorschauphase verwaltet Microsoft alle Failoverereignisse, die sich auf GZRS- und RA-GZRS-Konten auswirken.

Da Daten asynchron in die sekundären Region repliziert werden, kann ein Fehler, der sich auf die primäre Region auswirkt, zu Datenverlusten führen, wenn die primäre Region nicht wiederhergestellt werden kann. Das Intervall zwischen den letzten Schreibvorgängen in der primären Region und dem letzten Schreibvorgang in der sekundären Region wird als RPO (Recovery Point Objective) bezeichnet. Die RPO gibt den Zeitpunkt an, auf den Daten wiederhergestellt werden können. Azure Storage weist normalerweise einen RPO-Wert von weniger als 15 Minuten auf, aber es gibt derzeit keine SLA zur Dauer der Replikation in die sekundäre Region.

Der RTO-Wert (Recovery Time Objective) ist eine Kennzahl dafür, wie lange es dauert, bis das Failover ausgeführt wurde und das Speicherkonto wieder online ist. Die Kennzahl gibt die von Azure für die Ausführung des Failovers benötigte Zeit für das Ändern der primären DNS-Einträge an, sodass sie auf den sekundären Standort verweisen.

## <a name="migrate-a-storage-account-to-gzrs-or-ra-gzrs"></a>Migrieren eines Speicher Kontos zu GZRS oder RA-GZRS

Sie können jedes vorhandenes Speicherkonto zu GZRS oder RA-GZRS migrieren. Die Migration von einem vorhandenen ZRS-Konto zu GZRS oder RA-GZRS ist einfach, während die Migration von einem LRS-, GRS-oder RA-GRS-Konto komplexer ist. In den folgenden Abschnitten wird beschrieben, wie die Migration in den einzelnen Fällen erfolgt.

### <a name="migrating-from-a-zrs-account"></a>Migrieren von einem ZRS-Konto

Um ein vorhandenes ZRS-Konto in RA-GZRS zu konvertieren, verwenden Sie das Cmdlet [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount), um die SKU für das Konto zu ändern. Denken Sie daran, die Platzhalterwerte durch Ihre eigenen Werte zu ersetzen:

```powershell
Set-AzStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -SkuName "Standard_RAGZRS"
```

### <a name="migrating-from-an-lrs-grs-or-ra-grs-account"></a>Migrieren von einem LRS-, GRS- oder RA-GRS-Konto

Es gibt zwei Optionen für die Migration von einem LRS-, GRS- oder RA-GRS-Konto zu GZRS oder RA-GZRS:

- Sie können Daten manuell aus einem vorhandenen Konto in ein neues GZRS- oder RA-GZRS-Konto kopieren oder verschieben.
- Sie können auch eine Livemigration anfordern.

#### <a name="perform-a-manual-migration"></a>Ausführen einer manuellen Migration

Wenn die Migration zu einem bestimmten Zeitpunkt abgeschlossen sein muss, sollten Sie eine manuelle Migration in Erwägung ziehen. Eine manuelle Migration bietet mehr Flexibilität als eine Livemigration. Bei einer manuellen Migration können Sie den zeitlichen Ablauf steuern.

Um Daten von einem bestehenden Konto manuell zu einem GZRS- oder RA-GZRS-Konto zu migrieren, verwenden Sie ein Tool, das Daten effizient kopieren kann. Beispiele hierfür sind:

- Verwenden Sie ein Hilfsprogramm wie AzCopy oder ein zuverlässiges Tool von einem Drittanbieter. Weitere Informationen zu AzCopy finden Sie unter [Erste Schritte mit AzCopy](storage-use-azcopy-v10.md).
- Wenn Sie mit Hadoop oder HDInsight vertraut sind, fügen Sie sowohl die Quell- als auch die Zielkonten an Ihren Cluster an. Danach parallelisieren Sie den Datenkopiervorgang mit einem Tool wie etwa DistCp.
- Verwenden Sie eine der Azure Storage-Clientbibliotheken, um eigene Tools zu erstellen.

#### <a name="perform-a-live-migration"></a>Ausführen einer Livemigration

Eine manuelle Migration kann zu Ausfallzeiten der Anwendung führen. Wenn Ihre Anwendung eine hohe Verfügbarkeit erfordert, bietet Microsoft auch eine Option für eine Livemigration. Eine Livemigration ist eine direkte Migration ohne Ausfallzeiten.

Während einer Livemigration können Sie Ihr Speicherkonto weiter verwenden, während Ihre Daten zwischen den Quell- und Zielkonten migriert werden. Während der Livemigration erfüllt Ihr Konto weiterhin seine SLA für Dauerhaftigkeit und Verfügbarkeit. Es entstehen keine Ausfallzeiten oder Datenverluste durch die Livemigration.

Nur Speicherkonten vom Typ „Allgemein v2“ unterstützen GZRS/RA-GZRS. Bevor Sie also eine Anforderung für eine Livemigration zu GZRS/RA-GZRS übermitteln, sollten Sie Ihre Konten auf „Allgemein v2“ upgraden. Weitere Informationen finden Sie unter [Übersicht über Azure Storage-Konten](https://docs.microsoft.com/azure/storage/common/storage-account-overview) und [Durchführen eines Upgrades auf ein Speicherkonto vom Typ „Universell v2“](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade).

Nach Abschluss der Migration wird die Replikationseinstellung des Speicherkontos auf **Geozonenredundanter Speicher (GZRS)** oder **Georedundanter Speicher mit Lesezugriff (RA-GZRS)** aktualisiert. Dienstendpunkte, Zugriffsschlüssel, SAS (Shared Access Signatures) und andere Konfigurationsoption für Konten bleiben unverändert und funktionsfähig.

Beachten Sie die folgenden Einschränkungen bei Livemigrationen:

- Auch wenn Microsoft auf Ihre Anforderung zur Livemigration sofort reagiert, gibt es keine Garantie, zu welchem Zeitpunkt eine Livemigration abgeschlossen sein wird. Wenn Ihre Daten bis zu einem bestimmten Datum zu GZRS oder RA-GZRS migriert werden müssen, empfiehlt Microsoft stattdessen die Durchführung einer manuellen Migration. Grundsätzlich gilt: Je mehr Daten in Ihrem Konto vorhanden sind, desto länger dauert die Migration dieser Daten.
- Ihr Konto muss Daten enthalten.
- Sie können Daten nur innerhalb der gleichen Region migrieren.
- Nur Standard-Speicherkontotypen unterstützen die Livemigration. Premium-Speicherkonten müssen manuelle migriert werden.
- Die Livemigration von einem GZRS- oder RA-GZRS-Konto zu einem LRS-, GRS- oder RA-GRS-Konto wird nicht unterstützt. Sie müssen diese Daten manuell in ein neues oder vorhandenes Speicherkonto verschieben.
- Sie können eine Livemigration von RA-GRS zu RA-GZRS anfordern. Das Migrieren von RA-GRS zu GZRS wird jedoch nicht unterstützt. In diesem Fall müssen Sie eine Livemigration zu RA-GZRS anfordern und dann das Speicherkonto manuell für die Verwendung von GZRS konvertieren.
- Verwaltete Datenträger unterstützen nur LRS und können nicht zu GZRS oder RA-GZRS migriert werden. Informationen zur Integration in Verfügbarkeitsgruppen finden Sie unter [Einführung in verwaltete Azure-Datenträger](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#integration-with-availability-sets).
- Sie können Momentaufnahmen und Images für SSD Standard Managed Disks in HDD Standard-Speicher speichern und zwischen [LRS-, ZRS-, GZRS- und RA-GZRS-Optionen](https://azure.microsoft.com/pricing/details/managed-disks/) wählen.
- Konten, die große Dateifreigaben enthalten, werden für GZRS nicht unterstützt.

Verwenden Sie das [Azure-Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview), um eine Livemigration anzufordern. Wählen Sie im Portal das Speicherkonto für die Migration zu GZRS oder RA-GZRS aus, und befolgen Sie diese Anweisungen:

1. Wählen Sie **Neue Supportanfrage** aus.
2. Geben Sie die **Grundlagen** basierend auf den Informationen Ihres Kontos an. Wählen Sie im Abschnitt **Dienst** die Option **Speicherkontoverwaltung** aus, und geben Sie das zu migrierende Konto an.
3. Klicken Sie auf **Weiter**.
4. Geben Sie im Abschnitt **Problem** die folgenden Werte an:
    - **Schweregrad**: Behalten Sie den Standardwert bei.
    - **Problemtyp**: Wählen Sie **Datenmigration** aus.
    - **Kategorie**: Wählen Sie **Innerhalb einer Region zu (RA-)GZRS migrieren** aus.
    - **Titel**: Geben Sie einen aussagekräftigen Titel ein, z. B. **(RA-)GZRS-Kontomigration**.
    - **Details**: Geben Sie im Feld **Details** weitere Informationen ein, z. B. „Ich möchte von [LRS, GRS] in der Region ‚\_\_‘ zu GZRS migrieren“. Oder: „Ich möchte von [LRS, RA-GRS] in der Region \_\_ zu RA-GZRS migrieren“.
5. Klicken Sie auf **Weiter**.
6. Überprüfen Sie, ob die Kontaktinformationen auf dem Blatt **Kontaktinformationen** korrekt sind.
7. Klicken Sie auf **Erstellen**.

Ein Supportmitarbeiter setzt sich mit Ihnen in Verbindung, um Unterstützung bereitzustellen.

## <a name="see-also"></a>Weitere Informationen

- [Azure Storage-Replikation](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
- [Lokal redundanter Speicher (LRS): Kostengünstige Datenredundanz für Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs)
- [Zonenredundanter Speicher (ZRS): Hochverfügbare Azure Storage-Anwendungen](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) 
