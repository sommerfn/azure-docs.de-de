---
title: Anpassen von Bewertungen für Azure Migrate-Serverbewertung | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie mit der Azure Migrate-Serverbewertung erstellte Bewertungen anpassen.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 2cfac978b0a5af20e9e2fa1e32a7361488f20fbe
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234266"
---
# <a name="customize-an-assessment"></a>Anpassen einer Bewertung

Dieser Artikel beschreibt, wie Sie mit der Azure Migrate-Serverbewertung erstellte Bewertungen anpassen.

[Azure Migrate](migrate-services-overview.md) ist ein zentraler Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration Ihrer lokalen Apps und Workloads sowie von VMs in der privaten/öffentlichen Cloud zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs) bereit.

Mit dem Tool Azure Migrate-Serverbewertung können Sie Bewertungen für lokale VMware-VMs und Hyper-V-VMs in Vorbereitung auf die Migration zu Azure erstellen.

## <a name="about-assessments"></a>Informationen zu Bewertungen

Mit der Azure Migrate-Serverbewertung können zwei Arten von Bewertungen ausgeführt werden.

**Bewertung** | **Details** | **Daten**
--- | --- | ---
**Leistungsbasiert** | Bewertungen basierend auf gesammelten Leistungsdaten | **Empfohlene VM-Größe**: Basierend auf CPU- und Arbeitsspeicher-Nutzungsdaten<br/><br/> **Empfohlener Datenträgertyp (Verwalteter Datenträger vom Typ Standard oder Premium)** : Basierend auf IOPS und Durchsatz der lokalen Datenträger
**Wie lokal** | Bewertungen basierend auf lokaler Größenanpassung | **Empfohlene VM-Größe**: Basierend auf der Größe der lokalen VM<br/><br> **Empfohlener Datenträgertyp**: Basierend auf der für die Bewertung ausgewählten Speichertypeinstellung


## <a name="how-is-an-assessment-done"></a>Wie wird eine Bewertung durchgeführt?

Eine Bewertung in der Azure Migrate-Serverbewertung erfolgt in drei Phasen. Die Bewertung beginnt mit einer Eignungsanalyse, gefolgt von einer Größenanpassung und abschließend einer Schätzung der monatlichen Kosten. Ein Computer gelangt nur in die nächste Phase, wenn er die vorherige besteht. Beispiel: Besteht ein Computer die Azure-Eignungsprüfung nicht, wird er als ungeeignet für Azure markiert, und die Größen- und Kostenschätzungen werden nicht durchgeführt. [Weitere Informationen.](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation)

## <a name="whats-in-an-assessment"></a>Was umfasst eine Bewertung?

**Eigenschaft** | **Details**
--- | ---
**Zielstandort** | Der Azure-Standort, zu dem die Migration durchgeführt werden soll.<br/> Die Serverbewertung unterstützt derzeit die folgenden Regionen: „Australien, Osten“, „Australien, Südosten“, „Brasilien, Süden“, „Kanada, Mitte“, „Kanada, Osten“, „Indien, Mitte“, „USA, Mitte“, „China, Osten“, „China, Norden“, „Asien, Osten“, „USA, Osten“, „Deutschland, Mitte“, „Deutschland, Nordosten“, „USA, Osten 2“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Indien, Süden“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „US Gov Arizona“, „US Gov Texas“, „US Gov Virginia“, „USA, Westen-Mitte“, „Europa, Westen“, „Indien, Westen“, „USA, Westen“ und „USA, Westen 2“.
**Speichertyp** | Sie können diese Eigenschaft nutzen, um den Typ der Datenträger anzugeben, die Sie nach Azure verschieben möchten.<br/><br/> Für die Größenanpassung für die lokale Umgebung können Sie den Zieldatenträgertyp als „Managed Disks Premium“, „Managed Disks SSD Standard“ oder „Managed Disks HDD Standard“ angeben. Für die leistungsbasierte Größenanpassung können Sie den Zieldatenträgertyp als „Automatisch“, „Managed Disks Premium“, „Managed Disks HDD Standard“ oder „Managed Disks SSD Standard“ angeben.<br/><br/> Beim Angeben des Speichertyps als „Automatisch“ erfolgt die Datenträgerempfehlung basierend auf den Leistungsdaten der Datenträger (IOPS und Durchsatz). Wenn Sie den Speichertyp als Premium/Standard angeben, empfiehlt die Bewertung eine Datenträger-SKU innerhalb des ausgewählten Speichertyps. Wenn Sie eine Einzelinstanz-VM-SLA von 99,9% erreichen möchten, könnten Sie als Speichertyp „Managed Disks Premium“ angeben. Dadurch wird sichergestellt, dass für alle Datenträger in der Bewertung „Managed Disks Premium“ empfohlen wird. Azure
**Reservierte Instanzen (RI)** | Mit dieser Eigenschaft können Sie festlegen, ob Sie [reservierte Instanzen](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Azure haben. Die Kostenschätzungen in der Bewertung werden dann unter Berücksichtigung von RI-Rabatten durchgeführt. Reservierte Instanzen werden derzeit nur für das Angebot mit nutzungsbasierter Bezahlung in Azure Migrate unterstützt.
**Größenkriterium** | Das Kriterium, das zur Größenanpassung virtueller Computer für Azure verwendet werden soll. Sie können eine *leistungsbasierte* Größenanpassung vornehmen oder die Größe der virtuellen Computer *lokal* ändern, ohne den Leistungsverlauf zu berücksichtigen.
**Leistungsverlauf** | Die zur Bewertung der Leistungdaten der Computer zu berücksichtigende Dauer. Diese Eigenschaft ist nur anwendbar, wenn als Größenkriterium *leistungsbasiert* festgelegt ist.
**Perzentilwert der Nutzung** | Der für die Größenanpassung zu berücksichtigende Perzentilwert des festgelegten Leistungsbeispiels. Diese Eigenschaft ist nur anwendbar, wenn als Größe *leistungsbasiert* festgelegt ist.
**VM-Serie** |     Sie können die VM-Serie angeben, die Sie für die Größenanpassung berücksichtigen möchten. Wenn Sie beispielsweise eine Produktionsumgebung haben, die Sie nicht zu VMs der A-Serie in Azure migrieren möchten, können Sie die A-Serie von der Liste oder aus der Serie ausschließen, sodass die Größenanpassung nur in der ausgewählten Serie erfolgt.
**Komfortfaktor** | Bei der Azure Migrate-Serverbewertung wird während der Bewertung ein Puffer (Komfortfaktor) berücksichtigt. Dieser Puffer wird zusätzlich zu den Daten zur Computernutzung für VMs (CPU, Arbeitsspeicher, Datenträger und Netzwerk) angewendet. Beim Komfortfaktor geht es um Bereiche wie saisonale Nutzung, kurzer Leistungsverlauf und voraussichtliche zukünftige Zunahme der Nutzung.<br/><br/> Beispiel: Für einen virtuellen Computer mit zehn Kernen und einer Auslastung von 20 Prozent ergibt sich normalerweise ein virtueller Computer mit zwei Kernen. Bei einem Komfortfaktor von 2.0x ist das Ergebnis dagegen eine VM mit vier Kernen.
**Angebot** | Das [Azure-Angebot](https://azure.microsoft.com/support/legal/offer-details/), bei dem Sie registriert sind. Dies wird dann von Azure Migrate bei der Kostenschätzung entsprechend berücksichtigt.
**Währung** | Rechnungswährung.
**Rabatt (%)** | Alle abonnementspezifischen Rabatte, die Sie zusätzlich zum Azure-Angebot erhalten.<br/> Die Standardeinstellung ist 0 %.
**VM-Betriebszeit** | Wenn Ihre virtuellen Computer nicht rund um die Uhr in Azure aktiv sind, können Sie den Zeitraum angeben (Anzahl der Tage pro Monat und Anzahl der Stunden pro Tag), für den sie voraussichtlich ausgeführt werden, um eine entsprechende Kostenschätzung zu erhalten.<br/> Der Standardwert sind 31 Tage pro Monat und 24 Stunden pro Tag.
**Azure-Hybridvorteil** | Geben Sie an, ob Sie über die Software Assurance verfügen und den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-use-benefit/) nutzen können. Wenn diese Einstellung auf „Ja“ festgelegt ist, werden für virtuelle Windows-Computer Nicht-Windows-Azure-Preise veranschlagt. Die Standardeinstellung ist „Ja“.


## <a name="edit-assessment-properties"></a>Bearbeiten von Bewertungseigenschaften

Gehen Sie folgendermaßen vor, um Bewertungseigenschaften nach dem Erstellen einer Bewertung zu bearbeiten:

1. Klicken Sie im Azure Migrate-Projekt auf **Server**.
2. Klicken Sie in **Azure Migrate: Serverbewertung** auf den Bewertungenleistungsindikator.
3. Klicken Sie in **Bewertung** auf die relevante Bewertung und dann auf **Eigenschaften bearbeiten**.
5. Passen Sie die Bewertungseigenschaften in Übereinstimmung mit der obigen Tabelle an.
6. Klicken Sie auf **Speichern**, um die Bewertung zu aktualisieren.


Sie können Bewertungseigenschaften auch bearbeiten, wenn Sie eine Bewertung erstellen.


## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen](concepts-assessment-calculation.md) zur Berechnung von Bewertungen
