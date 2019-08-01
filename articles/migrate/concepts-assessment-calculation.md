---
title: Bewertungsberechnungen in Azure Migrate | Microsoft-Dokumentation
description: Bietet eine Übersicht über Bewertungsberechnungen im Azure Migrate-Dienst.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 87aa48c992b7887ce86c43cac29910dcc57b3e91
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234294"
---
# <a name="assessment-calculations"></a>Bewertungsberechnungen

[Azure Migrate](migrate-services-overview.md) ist ein zentraler Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration Ihrer lokalen Apps und Workloads sowie von Instanzen in der privaten/öffentlichen Cloud zu Azure. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs) bereit.

Die Serverbewertung ist ein Tool in Azure Migrate, das lokale Server für die Migration in Azure bewertet. Dieser Artikel enthält Informationen dazu, wie Bewertungen berechnet werden.

## <a name="whats-in-an-assessment"></a>Was umfasst eine Bewertung?

**Eigenschaft** | **Details**
--- | ---
**Zielstandort** | Der Azure-Standort, zu dem die Migration durchgeführt werden soll.<br/> Die Serverbewertung unterstützt derzeit die folgenden Regionen: „Australien, Osten“, „Australien, Südosten“, „Brasilien, Süden“, „Kanada, Mitte“, „Kanada, Osten“, „Indien, Mitte“, „USA, Mitte“, „China, Osten“, „China, Norden“, „Asien, Osten“, „USA, Osten“, „Deutschland, Mitte“, „Deutschland, Nordosten“, „USA, Osten 2“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Indien, Süden“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „US Gov Arizona“, „US Gov Texas“, „US Gov Virginia“, „USA, Westen-Mitte“, „Europa, Westen“, „Indien, Westen“, „USA, Westen“ und „USA, Westen 2“.
**Speichertyp** | Sie können diese Eigenschaft nutzen, um den Typ der Datenträger anzugeben, die Sie nach Azure verschieben möchten. <br/><br/> Für die Größenanpassung für die lokale Umgebung können Sie den Zieldatenträgertyp als „Managed Disks Premium“, „Managed Disks SSD Standard“ oder „Managed Disks HDD Standard“ angeben. Für die leistungsbasierte Größenanpassung können Sie den Zieldatenträgertyp als „Automatisch“, „Managed Disks Premium“, „Managed Disks HDD Standard“ oder „Managed Disks SSD Standard“ angeben.<br/><br/> Beim Angeben des Speichertyps als „Automatisch“ erfolgt die Datenträgerempfehlung basierend auf den Leistungsdaten der Datenträger (IOPS und Durchsatz). Wenn Sie den Speichertyp als Premium/Standard angeben, empfiehlt die Bewertung eine Datenträger-SKU innerhalb des ausgewählten Speichertyps. Wenn Sie eine Einzelinstanz-VM-SLA von 99,9 % erreichen möchten, könnten Sie als Speichertyp „Managed Disks Premium“ angeben. Dadurch wird sichergestellt, dass für alle Datenträger in der Bewertung „Managed Disks Premium“ empfohlen wird. Hinweis: Azure Migrate unterstützt nur verwaltete Datenträger für die Migrationsbewertung.
**Reservierte Instanzen (RI)** | Mit dieser Eigenschaft können Sie festlegen, ob Sie [reservierte Instanzen](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Azure haben. Die Kostenschätzungen in der Bewertung werden dann unter Berücksichtigung von RI-Rabatten durchgeführt. Reservierte Instanzen werden derzeit nur für das Angebot mit nutzungsbasierter Bezahlung in Azure Migrate unterstützt.
**Größenkriterium** | Das Kriterium, das zur Größenanpassung virtueller Computer für Azure verwendet werden soll. Sie können eine *leistungsbasierte* Größenanpassung vornehmen oder die Größe der virtuellen Computer *lokal* ändern, ohne den Leistungsverlauf zu berücksichtigen.
**Leistungsverlauf** | Die zur Bewertung der Leistungdaten der Computer zu berücksichtigende Dauer. Diese Eigenschaft ist nur anwendbar, wenn als Größenkriterium *„leistungsbasiert“* festgelegt ist.
**Perzentilwert der Nutzung** | Der für die Größenanpassung zu berücksichtigende Perzentilwert des festgelegten Leistungsbeispiels. Diese Eigenschaft ist nur anwendbar, wenn als Größe *„leistungsbasiert“* festgelegt ist.
**VM-Serie** |     Sie können die VM-Serie angeben, die Sie für die Größenanpassung berücksichtigen möchten. Wenn Sie beispielsweise eine Produktionsumgebung haben, die Sie nicht zu VMs der A-Serie in Azure migrieren möchten, können Sie die A-Serie von der Liste oder aus der Serie ausschließen, sodass die Größenanpassung nur in der ausgewählten Serie erfolgt.
**Komfortfaktor** | Bei der Azure Migrate-Serverbewertung wird während der Bewertung ein Puffer (Komfortfaktor) berücksichtigt. Dieser Puffer wird zusätzlich zu den Daten zur Computernutzung für VMs (CPU, Arbeitsspeicher, Datenträger und Netzwerk) angewendet. Beim Komfortfaktor geht es um Bereiche wie saisonale Nutzung, kurzer Leistungsverlauf und voraussichtliche zukünftige Zunahme der Nutzung.<br/><br/> Beispiel: Für einen virtuellen Computer mit zehn Kernen und einer Auslastung von 20 Prozent ergibt sich normalerweise ein virtueller Computer mit zwei Kernen. Bei einem Komfortfaktor von 2.0x ist das Ergebnis dagegen eine VM mit vier Kernen.
**Angebot** | Das [Azure-Angebot](https://azure.microsoft.com/support/legal/offer-details/), bei dem Sie registriert sind. Dies wird dann von Azure Migrate bei der Kostenschätzung entsprechend berücksichtigt.
**Währung** | Rechnungswährung.
**Rabatt (%)** | Alle abonnementspezifischen Rabatte, die Sie zusätzlich zum Azure-Angebot erhalten.<br/> Die Standardeinstellung ist 0 %.
**VM-Betriebszeit** | Wenn Ihre virtuellen Computer nicht rund um die Uhr in Azure aktiv sind, können Sie den Zeitraum angeben (Anzahl der Tage pro Monat und Anzahl der Stunden pro Tag), für den sie voraussichtlich ausgeführt werden, um eine entsprechende Kostenschätzung zu erhalten.<br/> Der Standardwert sind 31 Tage pro Monat und 24 Stunden pro Tag.
**Azure-Hybridvorteil** | Geben Sie an, ob Sie über die Software Assurance verfügen und den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-use-benefit/) nutzen können. Wenn diese Einstellung auf „Ja“ festgelegt ist, werden für virtuelle Windows-Computer Nicht-Windows-Azure-Preise veranschlagt. Die Standardeinstellung ist „Ja“.

## <a name="how-are-assessments-calculated"></a>Wie werden Bewertungen berechnet?

Eine Bewertung in der Azure Migrate-Serverbewertung wird anhand der Metadaten berechnet, die über die lokalen Server gesammelt wurden. Die Bewertungsberechnung erfolgt in drei Schritten: sie beginnt für jeden Server mit der Azure-Eignungsanalyse, gefolgt von der Dimensionierung und schließlich einer monatlichen Kostenschätzung. Ein Server gelangt nur in die nächste Phase, wenn er die vorherige besteht. Beispiel: Besteht ein Server die Azure-Eignungsprüfung nicht, wird er als ungeeignet für Azure markiert, und die Größen- und Kostenschätzungen werden nicht durchgeführt.

## <a name="azure-suitability-analysis"></a>Azure-Eignungsanalyse

Nicht alle Computer sind für die Ausführung in Azure geeignet. Die Azure Migrate-Serverbewertung bewertet die Eignung jedes lokalen Computers für die Migration zu Azure und stuft die bewerteten Computer in eine der folgenden Eignungskategorien ein:
- **Bereit für Azure:** Der Computer kann ohne Änderungen zu Azure migriert werden. Er wird in Azure mit vollständiger Azure-Unterstützung gestartet.
- **Bedingt bereit für Azure:** Der Computer kann in Azure gestartet werden, verfügt jedoch möglicherweise nicht über die vollständige Azure-Unterstützung. Beispielsweise wird ein Computer mit einer älteren Version des Windows Server-Betriebssystems in Azure nicht unterstützt. Gehen Sie bei der Migration dieser Computer zu Azure vorsichtig vor, und befolgen Sie die in der Bewertung empfohlene Korrekturanleitung, um die Bereitschaftsprobleme vor der Migration zu beheben.
- **Nicht bereit für Azure:** Der Computer wird in Azure nicht gestartet. Ein lokaler Computer mit einem angefügten Datenträger mit einer Größe von mehr als 64 TB kann beispielsweise in Azure nicht gehostet werden. Sie müssen die in der Bewertung empfohlene Korrekturanleitung befolgen, um die Bereitschaftsprobleme vor der Migration zu Azure zu beheben. Für Computer, die als nicht bereit für Azure markiert sind, werden keine Größen- und Kostenschätzungen durchgeführt.
- **Bereitschaft unbekannt:** Azure Migrate konnte die Bereitschaft des Computers aufgrund unzureichender Metadaten aus der lokalen Umgebung nicht finden.

Die Azure Migrate-Serverbewertung überprüft die Computereigenschaften und das Gastbetriebssystem, um die Azure-Bereitschaft des lokalen Computers zu ermitteln.

### <a name="machine-properties"></a>Computereigenschaften

Die Serverbewertung überprüft die folgenden Eigenschaften des lokalen virtuellen Computers, um zu ermitteln, ob der in Azure ausgeführt werden kann.

**Eigenschaft** | **Details** | **Azure-Bereitschaftsstatus**
--- | --- | ---
**Starttyp** | Azure unterstützt virtuelle Computer mit dem Starttyp BIOS, jedoch nicht UEFI. | Bedingt bereit, wenn der Starttyp UEFI ist.
**Kerne** | Die Anzahl der Kerne in den Computern darf maximal der Anzahl der für einen virtuellen Azure-Computer unterstützten Kerne (128 Kerne) entsprechen.<br/><br/> Wenn der Leistungsverlauf verfügbar ist, berücksichtigt Azure Migrate die genutzten Kerne beim Vergleich. Wenn in den Bewertungseinstellungen ein Komfortfaktor festgelegt ist, wird die Anzahl der genutzten Kerne mit dem Komfortfaktor multipliziert.<br/><br/> Wenn kein Leistungsverlauf vorhanden ist, verwendet Azure Migrate die zugeordneten Kerne ohne Anwendung des Komfortfaktors. | Bereit, wenn kleiner als die Grenzwerte oder gleich diesen.
**Memory** | Die Größe des Computerarbeitsspeichers darf maximal dem zulässigen Arbeitsspeicher (3.892GB bei Azure M-Serie Standard_M128m&nbsp;<sup>2</sup>) für einen virtuellen Azure-Computer entsprechen. [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)<br/><br/> Wenn der Leistungsverlauf verfügbar ist, berücksichtigt Azure Migrate den genutzten Arbeitsspeicher beim Vergleich. Wenn ein Komfortfaktor festgelegt ist, wird der genutzte Arbeitsspeicher mit dem Komfortfaktor multipliziert.<br/><br/> Wenn kein Verlauf vorhanden ist, wird der zugeordnete Arbeitsspeicher ohne Anwendung des Komfortfaktors verwendet.<br/><br/> | Bereit, wenn innerhalb der Grenzen.
**Speicherdatenträger** | Die zugeteilte Größe eines Datenträgers darf höchstens 32 TB betragen.<br/><br/> An den Computer dürfen einschließlich des Betriebssystem-Datenträgers höchstens 65 Datenträger angefügt sein. | Bereit, wenn innerhalb der Grenzen.
**Netzwerk** | An einen Computer dürfen höchstens 32 NICs angefügt sein. | Bereit, wenn innerhalb der Grenzen.

### <a name="guest-operating-system"></a>Gastbetriebssystem
Neben den VM-Eigenschaften untersucht die Azure Migrate-Serverbewertung auch das Gastbetriebssystem der Computer, um festzustellen, ob sie auf Azure ausgeführt werden können.

> [!NOTE]
> Für VMware-VMs verwendet die Azure Migrate-Serverbewertung das für die VM in vCenter Server angegebene Betriebssystem, um die Gastbetriebssystemanalyse durchzuführen. Für Linux-VMs, die auf VMware ausgeführt werden, wird derzeit nicht genau die Kernelversion des Gastbetriebsystems identifiziert.

Zur Ermittlung der Azure-Bereitschaft basierend auf dem Betriebssystem wird von der Azure Migrate-Serverbewertung folgende Logik verwendet.

**Betriebssystem** | **Details** | **Azure-Bereitschaftsstatus**
--- | --- | ---
Windows Server 2016 und alle SPs | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2012 R2 und alle SPs | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2012 und alle SPs | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2008 R2 mit allen SPs | Azure bietet vollständige Unterstützung.| Bereit für Azure
Windows Server 2008 (32-Bit und 64-Bit) | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2003, 2003 R2 | Der Unterstützungszeitraum für diese Betriebssysteme ist abgelaufen. Für die Unterstützung in Azure wird eine benutzerdefinierte Supportvereinbarung ([Custom Support Agreement, CSA](https://aka.ms/WSosstatement)) benötigt. | Bedingt bereit für Azure, erwägen Sie ein Upgrade des Betriebssystems vor der Migration zu Azure.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Der Unterstützungszeitraum für diese Betriebssysteme ist abgelaufen. Der Computer kann in Azure gestartet werden, es wird jedoch keine Unterstützung des Betriebssystems bereitgestellt. | Bedingt bereit für Azure, es empfiehlt sich ein Upgrade des Betriebssystems vor der Migration zu Azure.
Windows Client 7, 8 und 10 | Azure bietet [nur mit Visual Studio-Abonnement](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) Unterstützung. | Bedingt bereit für Azure
Windows 10 Pro Desktop | Azure bietet Unterstützung mit [mehrinstanzenfähigen Hostingrechten](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment). | Bedingt bereit für Azure
Windows Vista, XP Professional | Der Unterstützungszeitraum für diese Betriebssysteme ist abgelaufen. Der Computer kann in Azure gestartet werden, es wird jedoch keine Unterstützung des Betriebssystems bereitgestellt. | Bedingt bereit für Azure, es empfiehlt sich ein Upgrade des Betriebssystems vor der Migration zu Azure.
Linux | Azure empfiehlt diese [Linux-Betriebssysteme](../virtual-machines/linux/endorsed-distros.md). Andere Linux-Betriebssysteme können in Azure gestartet werden. Es empfiehlt sich jedoch ein Upgrade des jeweiligen Betriebssystems auf eine unterstützte Version vor der Migration zu Azure. | Bereit für Azure, wenn die Version unterstützt wird.<br/><br/>Bedingt bereit, wenn die Version nicht unterstützt wird.
Andere Betriebssysteme<br/><br/> Beispielsweise Oracle Solaris, Apple Mac OS, FreeBSD usw. | Azure unterstützt diese Betriebssysteme nicht. Der Computer kann in Azure gestartet werden, es wird jedoch keine Unterstützung des Betriebssystems bereitgestellt. | Bedingt bereit für Azure, es empfiehlt sich die Installation eines unterstützten Betriebssystems vor der Migration zu Azure.  
In vCenter Server als **Sonstige** angegebenes Betriebssystem | In diesem Fall kann Azure Migrate das Betriebssystem nicht identifizieren. | Bereitschaft unbekannt. Stellen Sie sicher, dass das auf dem virtuellen Computer ausgeführte Betriebssystem in Azure unterstützt wird.
32-Bit-Betriebssysteme | Der Computer kann in Azure gestartet werden, Azure bietet jedoch möglicherweise keine vollständige Unterstützung. | Bedingt bereit für Azure, ziehen Sie vor der Migration zu Azure das Aktualisieren des Computerbetriebssystems von 32 Bit auf 64 Bit in Betracht.

## <a name="sizing"></a>Festlegen der Größe

Nachdem ein Computer als bereit für Azure vorgemerkt wurde, führt die Serverbewertung eine Größenanpassung durch, bei der die geeignete Azure-VM und Festplatten-SKU für die lokale VM identifiziert wird. Die Größenempfehlungen variieren je nach den angegebenen Bewertungseigenschaften.

- Wenn Die Bewertung ein *leistungsbasiertes Größenkriterium* verwendet, berücksichtigt Azure Migrate den Leistungsverlauf des Computers, um die VM-Größe und den Datenträgertyp in Azure zu ermitteln. Diese Methode ist besonders hilfreich, wenn Sie die lokalen virtuellen Computer überbelegt haben, die Auslastung jedoch gering ist, und Sie die Größe der virtuellen Computer in Azure optimal anpassen möchten, um Kosten zu sparen. Diese Methode wird Ihnen helfen, die Größen während der Migration zu optimieren.
- Wenn Sie die Leistungsdaten der VM-Skalierung nicht prüften wollen und die lokalen Computer unverändert zu Azure migriert werden soll, können Sie das Größenkriterium *„Wie lokal“* angeben. Die Serverbewertung legt dann die Größe der virtuellen Computer basierend auf der lokalen Konfiguration ohne Berücksichtigung der Nutzungsdaten fest. Die Größenanpassung für den Datenträger erfolgt in diesem Fall basierend auf dem Storage-Typ, den Sie in den Bewertungseigenschaften angeben (HDD Standard-Datenträger, SSD Standard-Datenträger oder Premium-Datenträger).

### <a name="performance-based-sizing"></a>Leistungsbasierte Größenanpassung

Bei der leistungsbasierten Größenanpassung prüft die Azure Migrate-Serverbewertung zunächst die an den virtuellen Computer angefügten Datenträger und dann die Netzwerkadapter und ordnet anschließend basierend auf den Computeanforderungen des lokalen virtuellen Computers einen Azure-VM-SKU zu. Die Azure Migrate-Appliance erstellt Profile von der lokalen Umgebung, um Leistungsdaten für CPU, Speicher, Festplatten und Netzwerkadapter zu sammeln.

**Leistungsdatensammlung**

- Für VMware-VMs sammelt die Azure Migrate-Appliance einen Echtzeit-Abtastpunkt im Abstand von 20 Sekunden. Für Hyper-V-VMs wird der Echtzeit-Abtastpunkt im Abstand von 30 Sekunden erfasst.
- Das Gerät führt dann einen Rollup für die alle 10 Minuten gesammelten Abtastpunkte aus und sendet den Maximalwert der letzten 10 Minuten an die Azure Migrate-Serverbewertung.
- Die Azure Migrate-Serverbewertung speichert alle 10-minütigen Abtastpunkte für den letzten Monat und identifiziert abhängig von den für den *Leistungsverlauf* und die *prozentuale Auslastung* angegebenen Bewertungseigenschaften den geeigneten Datenpunkt, der für die richtige Größe verwendet werden sollte. Wenn beispielsweise der Leistungsverlauf auf einen Tag festgelegt ist und die prozentuale Auslastung das 95. Quantil ist, dann verwendet sie die 10-minütigen Abtaastpunkte für den letzten Tag, sortiert sie in aufsteigender Reihenfolge und wählt das 95. Quantil für die richtige Größe aus.
- Der obige Wert wird dann mit dem Komfortfaktor multipliziert, um die effektiven Leistungsauslastungsdaten für jede Metrik (CPU-Auslastung, Speicherauslastung, Festplatten-IOPS (Lesen und Schreiben), Festplattendurchsatz (Lesen und Schreiben), Netzwerkdurchsatz (Ein- und Ausgabe)) zu erhalten, die die Appliance sammelt.
- Sobald der effektive Nutzungswert identifiziert ist, erfolgt die Berechnung, Speicherung und Netzwerkdimensionierung wie folgt:

**Speichergröße**: Azure Migrate versucht, jeden an den Computer angefügten Datenträger einem Datenträger in Azure zuzuordnen.

> [!NOTE]
> Azure Migrate: Die Serverbewertung unterstützt bei der Bewertung nur verwaltete Datenträger.

  - Die Lese- und Schreib-IOPS einer Festplatte werden addiert, um die insgesamt erforderlichen IOPS zu erhalten. Auf ähnliche Weise werden die Durchsatzwerte für Lesen und Schreiben addiert, um den Gesamtdurchsatz jeder Festplatte zu erhalten.
  - Wenn Sie den Speichertyp, basierend auf den effektiven IOPS- und Durchsatzwerten, als „automatisch“ angegeben haben, identifiziert die Azure Migrate-Serverbewertung dann, ob der Datenträger auf einem Standarddatenträger, einem SSD Standard-Datenträger oder einem Premium-Datenträger in Azure abgebildet werden soll. Wenn der Speichertyp als HDD Standard/SSD Standard/Premium-Datenträger eingerichtet ist, wird versucht, eine Datenträger-SKU innerhalb des ausgewählten Speichertyps (HDD Standard/SSD Standard/Premium-Datenträger) zu finden.
  - Wenn die Serverbewertung keinen Datenträger mit den erforderlichen IOPS- und Durchsatzwerten finden kann, wird der Computer als ungeeignet für Azure gekennzeichnet.
  - Wenn mehrere geeignete Datenträger gefunden werden, wählt die Serverbewertung diejenigen aus, die den Speicherort, der in den Bewertungseinstellungen angegebenen ist, unterstützen.
  - Falls mehrere geeignete Datenträger vorhanden sind, werden die mit den geringsten Kosten ausgewählt.
  - Wenn Leistungsdaten für eine Festplatte nicht verfügbar sind, werden die Konfigurationsdaten des Datenträgers (Datenträgergröße) verwendet, um einen SSD Standard-Datenträger in Azure zu finden.

**Netzwerkgröße**: Die Azure Migrate-Serverbewertung versucht, einen virtuellen Azure-Computer zu finden, der die Anzahl der an den lokalen Computer angefügten Netzwerkadapter und die für diese Netzwerkadapter erforderliche Leistung unterstützen kann.
    - Um die effektive Netzwerkleistung des lokalen virtuellen Computers zu ermitteln, aggregiert die Azure Migrate-Serverbewertung die vom Computer (Netzwerk ausgehend) pro Sekunde übertragenen Daten (MBit/s) für alle Netzwerkadapter und wendet den Komfortfaktor an. Mithilfe dieser Zahl wird ein virtueller Azure-Computer gesucht, der die erforderliche Netzwerkleistung unterstützen kann.
    - Neben der Netzwerkleistung wird zudem geprüft, ob der virtuelle Azure-Computer die erforderliche Anzahl der Netzwerkadapter unterstützen kann.
    - Wenn keine Netzwerkleistungsdaten verfügbar sind, wird bei der Größenanpassung für virtuelle Computer nur die Anzahl der Netzwerkadapter berücksichtigt.

**Computegröße**: Nach der Berechnung der Speicher- und Netzwerkanforderungen prüft die Azure Migrate-Serverbewertung die CPU- und Arbeitsspeicheranforderungen, um eine geeignete VM-Größe in Azure zu suchen.
    - Azure Migrate betrachtet die effektiv genutzten Kerne und den Speicher, um eine geeignete VM-Größe in Azure zu finden.
    - Wenn keine geeignete Größe gefunden wird, wird der Computer als ungeeignet für Azure gekennzeichnet.
    - Wenn eine geeignete Größe gefunden wird, wendet Azure Migrate die Speicher -und Netzwerkberechnungen an. Für die endgültige VM-Größenempfehlung werden dann noch Speicherort- und Tarifeinstellungen angewendet.
    - Wenn mehrere geeignete Azure-VM-Größen vorhanden sind, wird die mit den geringsten Kosten empfohlen.

### <a name="as-on-premises-sizing"></a>Größenanpassung vom Typ „Wie lokal“

Wenn Sie das Größenkriterium als *„Wie lokal“* angegeben haben, berücksichtigt die Azure Migrate-Serverbewertung den Leistungsverlauf der virtuellen Computer und Datenträger nicht und ordnet einer VM SKU basierend auf der lokal zugeordneten Größe zu. Analog dazu berücksichtigt Azure Migrate bei der Größenanpassung des Datenträgers den Storage-Typ, der in den Bewertungseigenschaften angegeben wurde (HDD Standard/SSD Standard oder Premium), und empfiehlt einen entsprechenden Datenträgertyp. Der Standardspeichertyp ist Premium.

## <a name="confidence-ratings"></a>Zuverlässigkeitsstufe
Jeder leistungsbasierten Bewertung in Azure Migrate wird eine Zuverlässigkeitsstufe zugeordnet, die zwischen ein (niedrigster Wert) bis fünf Sterne (höchster Wert) betragen kann.
- Die Zuverlässigkeitsstufe wird einer Bewertung auf der Grundlage der Verfügbarkeit von Datenpunkten zugeordnet, die zum Berechnen der Bewertung erforderlich sind.
- Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der von Azure Migrate bereitgestellten Größenempfehlungen besser einschätzen.
- Die Zuverlässigkeitsstufe gilt nicht für lokale Bewertungen.
- Die Azure Migrate-Serverbewertung benötigt für die leistungsbasierte Dimensionierung die folgenden Informationen:
    - Die Nutzungsdaten für CPU und VM-Arbeitsspeicher.
    - Außerdem werden für jeden an den virtuellen Computer angefügten Datenträger Informationen zu IOPS und Durchsatzdaten benötigt.
    - Analog dazu benötigt die Zuverlässigkeitsstufe für jeden Netzwerkadapter, der an einen virtuellen Computer angefügt ist, Informationen zu Netzwerk-E/A, um die leistungsbasierte Größenanpassung durchführen zu können.
    - Steht eine der oben aufgeführten Nutzungsangaben in vCenter Server nicht zur Verfügung, ist die Größenempfehlung unter Umständen nicht zuverlässig.

Die Zuverlässigkeitsstufe für die Bewertung ist abhängig davon, wie viele Datenpunkte verfügbar sind (in Prozent), wie es nachfolgend angegeben ist:

   **Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
   --- | ---
   0 % bis 20 % | Ein Stern
   21 % bis 40 % | Zwei Sterne
   41 % bis 60 % | Drei Sterne
   61 % bis 80 % | Vier Sterne
   81 % bis 100 % | Fünf Sterne

### <a name="low-confidence-ratings"></a>Geringe Zuverlässigkeitsstufe

Einige mögliche Gründe für eine niedrige Zuverlässigkeitsstufe einer Bewertung:

- Sie haben für den Zeitraum, für den Sie die Bewertung erstellen, kein Profil Ihrer Umgebung erstellt. Wenn Sie z.B. die Bewertung mit einer auf 1 Tag festgelegten Leistungsdauer erstellen, müssen Sie bis mindestens einen Tag nach dem Start der Ermittlung warten, bis alle Datenpunkte gesammelt sind.
- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, heruntergefahren. Wenn einige VMs für eine gewisse Zeit heruntergefahren wurden, kann die Azure Migrate-Serverbewertung für diesen Zeitraum keine Leistungsdaten sammeln.
- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, erstellt. Wenn Sie beispielsweise eine Bewertung für den Leistungsverlauf des letzten Monats erstellen, aber einige VMs erst vor einer Woche in der Umgebung erstellt wurden, dann ist der Leistungsverlauf der neuen VMs nicht für die gesamte Dauer abrufbar.

> [!NOTE]
> Bei einer Zuverlässigkeitsstufe von weniger als fünf Sternen wird empfohlen, mindestens einen Tag zu warten, damit die Appliance ein Profil der Umgebung erstellen kann. Führen Sie dann eine Neuberechnung der Bewertung durch. Wenn Sie dies nicht tun, ist die leistungsbasierte Größenanpassung möglicherweise nicht zuverlässig. Wir empfehlen, die Bewertung so zu ändern, dass sie als lokale Größenanpassung verwendet wird.

## <a name="monthly-cost-estimation"></a>Schätzung der monatlichen Kosten

Nach Abschluss der Größenempfehlungen berechnet Azure Migrate die Computer- und Speicherkosten nach der Migration.

- **Computekosten**: Azure Migrate berechnet anhand der empfohlenen Azure-VM-Größe mithilfe der Abrechnungs-API die monatlichen Kosten für den virtuellen Computer.
    - Bei der Berechnung werden Betriebssystem, Software Assurance, reservierte Instanzen, VM-Betriebszeit, Standort und Währungseinstellungen berücksichtigt.
    - Die Kosten aller Computer werden zusammengefasst, um die monatlichen Gesamtcomputekosten zu berechnen.
- **Speicherkosten**: Die monatlichen Speicherkosten für einen Computer werden berechnet, indem die monatlichen Kosten aller an den Computer angefügten Datenträger zusammengefasst werden.
    - Die Azure Migrate-Serverbewertung berechnet die gesamten monatlichen Speicherkosten durch Aggregieren der Speicherkosten aller Computer.
    - Derzeit werden bei der Berechnung keine Angebote berücksichtigt, die in den Bewertungseinstellungen angegeben sind.

Kosten werden in der Währung angezeigt, die in den Bewertungseinstellungen festgelegt ist.


## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine Bewertung für [VMware-VMs](tutorial-assess-vmware.md) oder [Hyper-V-VMs](tutorial-assess-hyper-v.md).
