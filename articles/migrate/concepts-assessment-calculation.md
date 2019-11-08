---
title: Bewertungsberechnungen in Azure Migrate | Microsoft-Dokumentation
description: Bietet eine Übersicht über Bewertungsberechnungen im Azure Migrate-Dienst.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: hamusa
ms.openlocfilehash: d72e5a6dea8b411b6214e7749b8993f9f5a6e7a8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73466919"
---
# <a name="assessment-calculations-in-azure-migrate"></a>Bewertungsberechnungen in Azure Migrate

[Azure Migrate](migrate-services-overview.md) bietet einen zentralen Hub zum Nachverfolgen der Ermittlung, Bewertung und Migration von lokalen Apps und Workloads. Außerdem werden Ihre privaten und öffentlichen Cloudinstanzen in Azure nachverfolgt. Der Hub stellt Azure Migrate-Tools für die Bewertung und Migration sowie Angebote von unabhängigen Drittanbietern (Independent Software Vendors, ISVs) bereit.

Die Serverbewertung ist ein Tool in Azure Migrate, das lokale Server für die Migration in Azure bewertet. Dieser Artikel enthält Informationen dazu, wie Bewertungen berechnet werden.

## <a name="whats-in-an-assessment"></a>Was umfasst eine Bewertung?

**Eigenschaft** | **Details**
--- | ---
**Zielstandort** | Gibt den Azure-Standort an, zu dem die Migration durchgeführt werden soll.<br/><br/>Die Serverbewertung unterstützt derzeit die folgenden Zielregionen: „Australien, Osten“, „Australien, Südosten“, „Brasilien, Süden“, „Kanada, Mitte“, „Kanada, Osten“, „Indien, Mitte“, „USA, Mitte“, „China, Osten“, „China, Norden“, „Asien, Osten“, „USA, Osten“, „Deutschland, Mitte“, „Deutschland, Nordosten“, „USA, Osten 2“, „Japan, Osten“, „Japan, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „USA, Norden-Mitte“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Indien, Süden“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „US Gov Arizona“, „US Gov Texas“, „US Gov Virginia“, „USA, Westen-Mitte“, „Europa, Westen“, „Indien, Westen“, „USA, Westen“ und „USA, Westen 2“.
**Speichertyp** | Gibt den Typ der Datenträger an, die Sie für die Speicherung in Azure verwenden möchten. <br/><br/> Für die Größenanpassung für die lokale Umgebung können Sie die Zieldatenträgertypen „Managed Disks Premium“, „Managed Disks SSD Standard“ oder „Managed Disks HDD Standard“ angeben. Für die leistungsorientierte Größenanpassung können Sie die Zieldatenträgertypen „Automatisch“, „Managed Disks Premium“, „Managed Disks HDD Standard“ oder „Managed Disks SSD Standard“ angeben. Bei Angabe des Speichertyps „Automatisch“ erfolgt die Datenträgerempfehlung basierend auf den Leistungsdaten der Datenträger: IOPS (Eingabe-/Ausgabevorgänge pro Sekunde) und Durchsatz. <br/><br/>Wenn Sie den Speichertyp Premium oder Standard angeben, empfiehlt die Bewertung eine Datenträger-SKU innerhalb des ausgewählten Speichertyps. Wenn Sie eine Einzelinstanz-VM-SLA von 99,9 % erreichen möchten, könnten Sie als Speichertyp „Managed Disks Premium“ angeben. Dadurch wird sichergestellt, dass für alle Datenträger in der Bewertung „Managed Disks Premium“ empfohlen wird. Hinweis: Azure Migrate unterstützt nur verwaltete Datenträger für die Migrationsbewertung.
**Reservierte Instanzen (RI)** | Mit dieser Eigenschaft können Sie [reservierte Instanzen](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Azure angeben. Für Kostenschätzungen in der Bewertung werden dann die RI-Rabatte berücksichtigt. Reservierte Instanzen werden derzeit nur für das Angebot mit nutzungsbasierter Bezahlung in Azure Migrate unterstützt.
**Größenkriterien** | Legt das Kriterium fest, das zur *Größenanpassung* virtueller Computer für Azure verwendet werden soll. Sie können eine *leistungsbasierte* Größenanpassung vornehmen oder die Größe der virtuellen Computer *lokal* ändern, ohne den Leistungsverlauf zu berücksichtigen.
**Leistungsverlauf** | Legt die zur Bewertung der Leistungsdaten der Computer zu berücksichtigende Dauer fest. Diese Eigenschaft ist nur anwendbar, wenn als Größenkriterium *leistungsbasiert* festgelegt ist.
**Perzentilwert der Nutzung** | Gibt den für die Größenanpassung zu berücksichtigenden Perzentilwert des festgelegten Leistungsbeispiels. Diese Eigenschaft ist nur anwendbar, wenn eine leistungsbasierte Größenanpassung ausgewählt wurde.
**VM-Serie** | Ermöglicht die Angabe der VM-Serie, die Sie für die Größenanpassung berücksichtigen möchten. Wenn Sie beispielsweise eine Produktionsumgebung haben, die Sie nicht zu VMs der A-Serie in Azure migrieren möchten, können Sie die A-Serie von der Liste oder aus der Serie ausschließen, sodass die Größenanpassung nur in der ausgewählten Serie erfolgt.
**Komfortfaktor** | Bei der Azure Migrate-Serverbewertung wird während der Bewertung ein Puffer (Komfortfaktor) berücksichtigt. Dieser Puffer wird zusätzlich zu den Daten zur Computernutzung für VMs (CPU, Arbeitsspeicher, Datenträger und Netzwerk) angewendet. Beim Komfortfaktor geht es um Bereiche wie saisonale Nutzung, kurzer Leistungsverlauf und voraussichtliche zukünftige Zunahme der Nutzung.<br/><br/> Beispiel: Für einen virtuellen Computer mit zehn Kernen und einer Auslastung von 20 Prozent ergibt sich normalerweise ein virtueller Computer mit zwei Kernen. Bei einem Komfortfaktor von 2.0x ist das Ergebnis dagegen eine VM mit vier Kernen.
**Angebot** | Zeigt das [Azure-Angebot](https://azure.microsoft.com/support/legal/offer-details/) an, bei dem Sie registriert sind. Dies wird dann von Azure Migrate bei der Kostenschätzung entsprechend berücksichtigt.
**Währung** | Zeigt die Abrechnungswährung für Ihr Konto an.
**Rabatt (%)** | Listet alle abonnementspezifischen Rabatte auf, die Sie zusätzlich zum Azure-Angebot erhalten. Die Standardeinstellung ist 0 %.
**VM-Betriebszeit** | Wenn Ihre virtuellen Computer nicht rund um die Uhr in Azure aktiv sind, können Sie den Zeitraum angeben (Anzahl der Tage pro Monat und Anzahl der Stunden pro Tag), für den sie voraussichtlich ausgeführt werden, um eine entsprechende Kostenschätzung zu erhalten. Der Standardwert sind 31 Tage pro Monat und 24 Stunden pro Tag.
**Azure-Hybridvorteil** | Gibt an, ob Sie über Software Assurance verfügen und den [Azure-Hybridvorteil](https://azure.microsoft.com/pricing/hybrid-use-benefit/) nutzen können. Wenn diese Einstellung auf „Ja“ festgelegt ist, werden für virtuelle Windows-Computer Nicht-Windows-Azure-Preise veranschlagt. Die Standardeinstellung ist „Ja“.

## <a name="how-are-assessments-calculated"></a>Wie werden Bewertungen berechnet?

Eine Bewertung in der Azure Migrate-Serverbewertung wird anhand der Metadaten berechnet, die über die lokalen Server gesammelt wurden. Wenn es sich bei der Ermittlungsquelle um einen Import mit einer CSV-Datei handelt, wird die Bewertung anhand der vom Benutzer bereitgestellten Metadaten für die Server berechnet. Die Bewertungsberechnung erfolgt in drei Schritten: Für jeden Server beginnt die Bewertung mit der Azure-Eignungsanalyse, gefolgt von der Größenanpassung und schließlich einer monatlichen Kostenschätzung. Ein Server gelangt nur in die nächste Phase, wenn er die vorherige besteht. Beispiel: Besteht ein Server die Azure-Eignungsprüfung nicht, wird er als ungeeignet für Azure markiert, und die Größen- und Kostenschätzungen werden für diesen Server nicht durchgeführt.

## <a name="azure-suitability-analysis"></a>Azure-Eignungsanalyse

Nicht alle Computer sind für die Ausführung in Azure geeignet. Bei der Serverbewertung werden alle lokalen Computer auf ihre Eignung für die Azure-Migration bewertet. Außerdem werden die einzelnen bewerteten Computer einer der folgenden Eignungskategorien zugewiesen:
- **Bereit für Azure**: Der Computer kann ohne Änderungen zu Azure migriert werden. Er wird in Azure mit vollständiger Azure-Unterstützung gestartet.
- **Bedingt bereit für Azure:** Der Computer kann ggf. in Azure gestartet werden, verfügt jedoch möglicherweise nicht über vollständigen Azure-Support. Beispielsweise wird ein Computer, auf dem eine ältere Version von Windows Server ausgeführt wird, in Azure nicht unterstützt. Gehen Sie bei der Migration dieser Computer zu Azure vorsichtig vor, und befolgen Sie die in der Bewertung empfohlene Korrekturanleitung, um die Bereitschaftsprobleme zu beheben.
- **Nicht bereit für Azure**: Der Computer wird nicht in Azure gestartet. Ein lokaler Computer mit einem angefügten Datenträger mit einer Größe von mehr als 64 TB (Terabyte) kann beispielsweise in Azure nicht gehostet werden. Sie müssen die in der Bewertung empfohlene Korrekturanleitung befolgen, um die Bereitschaftsprobleme vor der Migration des Computers zu Azure zu beheben. Für Computer, die als nicht bereit für Azure markiert sind, werden keine Größen- und Kostenschätzungen durchgeführt.
- **Bereitschaft unbekannt**: Azure Migrate konnte die Bereitschaft des Computers aufgrund unzureichender Metadaten aus der lokalen Umgebung nicht bestimmen.

Die Serverbewertung überprüft die Computereigenschaften und das Gastbetriebssystem, um die Azure-Bereitschaft des lokalen Computers zu bestimmen.

### <a name="machine-properties"></a>Computereigenschaften

Die Serverbewertung überprüft die folgenden Eigenschaften des lokalen virtuellen Computers, um zu ermitteln, ob dieser in Azure ausgeführt werden kann.

**Eigenschaft** | **Details** | **Azure-Bereitschaftsstatus**
--- | --- | ---
**Starttyp** | Azure unterstützt virtuelle Computer mit dem Starttyp BIOS, jedoch nicht UEFI. | Bedingt bereit, wenn der Starttyp UEFI ist.
**Kerne** | Die Anzahl der Kerne in den Computern darf maximal der Anzahl der für einen virtuellen Azure-Computer unterstützten Kerne (128) entsprechen.<br/><br/> Wenn der Leistungsverlauf verfügbar ist, berücksichtigt Azure Migrate die genutzten Kerne beim Vergleich. Wenn in den Bewertungseinstellungen ein Komfortfaktor festgelegt ist, wird die Anzahl der genutzten Kerne mit dem Komfortfaktor multipliziert.<br/><br/> Wenn kein Leistungsverlauf vorhanden ist, verwendet Azure Migrate die zugeordneten Kerne ohne Anwendung des Komfortfaktors. | Bereit, wenn kleiner als die Grenzwerte oder gleich diesen.
**Memory** | Die Größe des Computerarbeitsspeichers darf maximal dem zulässigen Arbeitsspeicher (3.892 GB bei Azure M-Serie Standard_M128m&nbsp;<sup>2</sup>) für einen virtuellen Azure-Computer entsprechen. [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/windows/sizes)<br/><br/> Wenn der Leistungsverlauf verfügbar ist, berücksichtigt Azure Migrate den genutzten Arbeitsspeicher beim Vergleich. Wenn ein Komfortfaktor festgelegt ist, wird der genutzte Arbeitsspeicher mit dem Komfortfaktor multipliziert.<br/><br/> Wenn kein Verlauf vorhanden ist, wird der zugeordnete Arbeitsspeicher ohne Anwendung des Komfortfaktors verwendet.<br/><br/> | Bereit, wenn innerhalb der Grenzen.
**Speicherdatenträger** | Die zugeteilte Größe eines Datenträgers darf höchstens 32 TB betragen. Obwohl Azure 64-TB-Datenträger mit SSD Ultra-Datenträgern unterstützt, überprüft die Serverbewertung von Azure Migrate noch die Datenträgergröße auf Überschreitung der 32 TB, da SSD Ultra noch nicht unterstützt wird. <br/><br/> An den Computer dürfen einschließlich des Betriebssystemdatenträgers höchstens 65 Datenträger angefügt sein. | Bereit, wenn innerhalb der Grenzen.
**Netzwerk** | An einen Computer dürfen maximal 32 Netzwerkschnittstellen (NICs) angefügt sein. | Bereit, wenn innerhalb der Grenzen.

### <a name="guest-operating-system"></a>Gastbetriebssystem
Neben den VM-Eigenschaften untersucht die Serverbewertung auch das Gastbetriebssystem der Computer, um zu ermitteln, ob sie auf Azure ausgeführt werden können.

> [!NOTE]
> Für VMware-VMs verwendet die Serverbewertung das für die VM in vCenter Server angegebene Betriebssystem, um die Gastbetriebssystemanalyse durchzuführen. Für Linux-VMs, die unter VMware ausgeführt werden, wird die Kernelversion des Gastbetriebssystems derzeit nicht genau identifiziert.

Zur Ermittlung der Azure-Bereitschaft basierend auf dem Betriebssystem wird von der Serverbewertung folgende Logik verwendet.

**Betriebssystem** | **Details** | **Azure-Bereitschaftsstatus**
--- | --- | ---
Windows Server 2016 und alle SPs | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2012 R2 und alle SPs | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2012 und alle SPs | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2008 R2 mit allen SPs | Azure bietet vollständige Unterstützung.| Bereit für Azure
Windows Server 2008 (32-Bit und 64-Bit) | Azure bietet vollständige Unterstützung. | Bereit für Azure
Windows Server 2003, 2003 R2 | Der Unterstützungszeitraum für diese Betriebssysteme ist abgelaufen. Für die Unterstützung in Azure wird eine benutzerdefinierte Supportvereinbarung ([Custom Support Agreement, CSA](https://aka.ms/WSosstatement)) benötigt. | Bedingt bereit für Azure. Ziehen Sie vor der Migration zu Azure ein Upgrade des Betriebssystems in Erwägung.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Diese Betriebssysteme haben das Ende des Supports überschritten. Der Computer startet möglicherweise in Azure, aber Azure stellt keine Betriebssystemunterstützung bereit. | Bedingt bereit für Azure. Es wird empfohlen, vor der Migration zu Azure ein Upgrade des Betriebssystems auszuführen.
Windows Client 7, 8 und 10 | Azure bietet [nur mit Visual Studio-Abonnement](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) Unterstützung. | Bedingt bereit für Azure
Windows 10 Pro Desktop | Azure bietet Unterstützung mit [mehrinstanzenfähigen Hostingrechten](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment). | Bedingt bereit für Azure
Windows Vista, XP Professional | Diese Betriebssysteme haben das Ende des Supports überschritten. Der Computer startet möglicherweise in Azure, aber Azure stellt keine Betriebssystemunterstützung bereit. | Bedingt bereit für Azure. Es wird empfohlen, vor der Migration zu Azure ein Upgrade des Betriebssystems auszuführen.
Linux | Azure empfiehlt diese [Linux-Betriebssysteme](../virtual-machines/linux/endorsed-distros.md). Andere Linux-Betriebssysteme können in Azure gestartet werden. Es empfiehlt sich jedoch ein Upgrade des jeweiligen Betriebssystems auf eine unterstützte Version vor der Migration zu Azure. | Bereit für Azure, wenn die Version unterstützt wird.<br/><br/>Bedingt bereit, wenn die Version nicht unterstützt wird.
Andere Betriebssysteme<br/><br/> Beispielsweise Oracle Solaris, Apple Mac OS, FreeBSD usw. | Azure unterstützt diese Betriebssysteme nicht. Der Computer startet möglicherweise in Azure, aber Azure stellt keine Betriebssystemunterstützung bereit. | Bedingt bereit für Azure. Es wird empfohlen, vor der Migration zu Azure ein unterstütztes Betriebssystem zu installieren.  
In vCenter Server als **Sonstige** angegebenes Betriebssystem | In diesem Fall kann Azure Migrate das Betriebssystem nicht identifizieren. | Bereitschaft unbekannt. Stellen Sie sicher, dass das auf dem virtuellen Computer ausgeführte Betriebssystem in Azure unterstützt wird.
32-Bit-Betriebssysteme | Der Computer kann in Azure gestartet werden, Azure bietet jedoch möglicherweise keine vollständige Unterstützung. | Bedingt bereit für Azure. Ziehen Sie vor der Migration zu Azure ein Upgrade des Computerbetriebssystems von 32 Bit auf 64 Bit in Betracht.

## <a name="sizing"></a>Festlegen der Größe

Nachdem ein Computer als bereit für Azure vorgemerkt wurde, gibt die Serverbewertung Empfehlungen für die Größenanpassung, bei der die geeignete Azure-VM und Festplatten-SKU für die lokale VM identifiziert wird. Die Empfehlungen variieren je nach den angegebenen Bewertungseigenschaften.

- Wenn die Bewertung eine *leistungsbasierte Dimensionierung* verwendet, berücksichtigt Azure Migrate den Leistungsverlauf des Computers, um die VM-Größe und den Datenträgertyp in Azure zu ermitteln. Bei Servern mit einer Ermittlungsquelle als Import werden die vom Benutzer angegebenen Werte für die Leistungsauslastung berücksichtigt. Diese Methode ist besonders hilfreich, wenn Sie die lokalen virtuellen Computer überbelegt haben, die Auslastung jedoch gering ist, und Sie die Größe der virtuellen Computer in Azure optimal anpassen möchten, um Kosten zu sparen. Diese Methode ist hilfreich, um die Größen bei der Migration zu optimieren.
- Wenn Sie die Leistungsdaten für die VM-Größe nicht berücksichtigen und stattdessen die lokalen Computer unverändert zu Azure übertragen möchten, können Sie die Größenkriterien auf *Wie lokal* festlegen. Anschließend wird die Größe der virtuellen Computer basierend auf der lokalen Konfiguration ohne Berücksichtigung der Nutzungsdaten von der Serverbewertung berechnet. Die Größenanpassung für den Datenträger erfolgt in diesem Fall basierend auf dem Speichertyp, den Sie in den Bewertungseigenschaften angeben (HDD Standard-, SSD Standard- oder Premium-Datenträger).

### <a name="performance-based-sizing"></a>Leistungsbasierte Größenanpassung

Bei der leistungsbasierten Größenanpassung beginnt die Serverbewertung mit den Datenträgern, die an den virtuellen Computer angefügt sind, und fährt danach mit den Netzwerkadaptern fort. Anschließend wird basierend auf den Computeanforderungen der lokalen VM eine Azure-VM-SKU zugeordnet. Die Azure Migrate-Appliance erstellt Profile der lokalen Umgebung, um Leistungsdaten für CPU, Speicher, Festplatten und Netzwerkadapter zu sammeln.

**Schritte zur Leistungsdatensammlung:**

1. Bei virtuellen VMware-Computern sammelt die Azure Migrate-Appliance Echtzeit-Abtastpunkte in einem 20-Sekunden-Intervall. Bei Hyper-V-VMs werden die Echtzeit-Abtastpunkte in einem Intervall von 30 Sekunden erfasst. Bei physischen Servern werden die Echtzeit-Abtastpunkte in einem Zeitabstand von 5 Minuten erfasst. 
2. Das Gerät führt einen Rollup für die alle 10 Minuten gesammelten Abtastpunkte aus und sendet den Maximalwert der letzten 10 Minuten an die Serverbewertung. 
3. Die Serverbewertung speichert alle 10-Minuten-Abtastpunkte des letzten Monats. Abhängig von den Bewertungseigenschaften, die für den *Leistungsverlauf* und die *prozentuale Auslastung* angegeben werden, wird der entsprechende Datenpunkt identifiziert, der für die richtige Größenanpassung verwendet werden soll. Wenn beispielsweise der Leistungsverlauf auf einen Tag festgelegt ist und die prozentuale Auslastung das 95. Perzentil ist, dann verwendet die Serverbewertung die 10-minütigen Abtastpunkte für den letzten Tag, sortiert sie in aufsteigender Reihenfolge und wählt das 95. Perzentil für die richtige Größe aus. 
4. Dieser Wert wird dann mit dem Komfortfaktor multipliziert, um die effektiven Leistungsauslastungsdaten für jede Metrik (CPU-Auslastung, Speicherauslastung, Festplatten-IOPS (Lesen und Schreiben), Festplattendurchsatz (Lesen und Schreiben), Netzwerkdurchsatz (Ein- und Ausgabe)) zu erhalten, die die Appliance sammelt.

Nachdem der Wert für die effektive Auslastung festgelegt wurde, werden Speicher-, Netzwerk- und Computegröße wie folgt behandelt.

> [!NOTE]
> Für Server, die über einen Import hinzugefügt wurden, werden die vom Benutzer bereitgestellten Leistungsdaten direkt für die richtige Dimensionierung verwendet.

**Speichergröße**: Azure Migrate versucht, jeden an den Computer angefügten Datenträger einem Datenträger in Azure zuzuordnen.

> [!NOTE]
> Die Azure Migrate-Serverbewertung unterstützt bei der Bewertung nur verwaltete Datenträger.

  - Die Serverbewertung addiert die Lese- und Schreib-IOPS eines Datenträgers, um die gesamten benötigten IOPS zu ermitteln. Ebenso werden die Werte für Lese- und Schreibdurchsatz addiert, um den gesamten Durchsatz jedes Datenträgers zu erhalten.
  - Wenn Sie als Speichertyp basierend auf den effektiven IOPS- und Durchsatzwerten „Automatisch“ angegeben haben, bestimmt die Serverbewertung dann, ob der Datenträger auf einem Standarddatenträger, einem SSD Standard-Datenträger oder einem Premium-Datenträger in Azure abgebildet werden soll. Wenn als Speichertyp HDD Standard-/SSD Standard-/Premium-Datenträger festgelegt wurde, wird versucht, eine Datenträger-SKU innerhalb des ausgewählten Speichertyps (HDD Standard-/SSD Standard-/Premium-Datenträger) zu finden.
  - Wenn die Serverbewertung keinen Datenträger mit den erforderlichen IOPS- und Durchsatzwerten finden kann, wird der Computer als ungeeignet für Azure gekennzeichnet.
  - Wenn die Serverbewertung mehrere geeignete Datenträger findet, wählt sie diejenigen aus, die den in den Bewertungseinstellungen angegebenen Speicherort unterstützen.
  - Falls mehrere geeignete Datenträger vorhanden sind, wählt die Serverbewertung den mit den geringsten Kosten aus.
  - Wenn für eine Festplatte keine Leistungsdaten verfügbar sind, werden die Konfigurationsdaten des Datenträgers (Datenträgergröße) verwendet, um einen SSD Standard-Datenträger in Azure zu finden.

**Netzwerkgröße**: Die Serverbewertung versucht, einen virtuellen Azure-Computer zu finden, der die Anzahl der an den lokalen Computer angefügten Netzwerkadapter und die für diese Netzwerkadapter erforderliche Leistung unterstützen kann.
- Um die effektive Netzwerkleistung des lokalen virtuellen Computers zu ermitteln, aggregiert die Azure Migrate-Serverbewertung die vom Computer (Netzwerk ausgehend) pro Sekunde übertragenen Daten (Mbit/s) für alle Netzwerkadapter und wendet den Komfortfaktor an. Mithilfe dieser Zahl wird ein virtueller Azure-Computer gesucht, der die erforderliche Netzwerkleistung unterstützen kann.
- Neben der Netzwerkleistung prüft die Serverbewertung zudem, ob der virtuelle Azure-Computer die erforderliche Anzahl der Netzwerkadapter unterstützen kann.
- Wenn keine Netzwerkleistungsdaten verfügbar sind, berücksichtigt die Serverbewertung bei der Größenanpassung für virtuelle Computer nur die Anzahl der Netzwerkadapter.

> [!NOTE]
> Die Angabe der Anzahl von Netzwerkadaptern wird für importierte Server derzeit nicht unterstützt.

**Computegröße**: Nach der Berechnung der Speicher- und Netzwerkanforderungen prüft die Serverbewertung die CPU- und Arbeitsspeicheranforderungen, um eine geeignete VM-Größe in Azure zu suchen.
- Azure Migrate betrachtet die effektiv genutzten Kerne und den Speicher, um eine geeignete VM-Größe in Azure zu finden.
- Wenn keine geeignete Größe gefunden wird, wird der Computer als ungeeignet für Azure gekennzeichnet.
- Wenn eine geeignete Größe gefunden wird, wendet Azure Migrate die Speicher -und Netzwerkberechnungen an. Für die endgültige VM-Größenempfehlung werden dann noch Standort- und Tarifeinstellungen angewandt.
- Wenn mehrere geeignete Azure-VM-Größen vorhanden sind, wird die mit den geringsten Kosten empfohlen.

### <a name="as-on-premises-sizing"></a>Größenanpassung vom Typ „Wie lokal“

Wenn Sie die Größenanpassung *Wie lokal* verwenden, wird der Leistungsverlauf der VMs und Datenträger von der Serverbewertung nicht berücksichtigt. Stattdessen wird eine VM-SKU in Azure basierend auf der lokal zugeordneten Größe zugewiesen. Analog dazu berücksichtigt die Serverbewertung bei der Größenanpassung des Datenträgers den Speichertyp, der in den Bewertungseigenschaften angegeben wurde (HDD Standard/SSD Standard oder Premium), und empfiehlt einen entsprechenden Datenträgertyp. Der Standardspeichertyp ist Premium.

## <a name="confidence-ratings"></a>Zuverlässigkeitsstufen
Jeder leistungsbasierten Bewertung in Azure Migrate wird eine Zuverlässigkeitsstufe zugeordnet, die zwischen ein (niedrigster Wert) bis fünf Sterne (höchster Wert) betragen kann.
- Die Zuverlässigkeitsstufe wird einer Bewertung auf der Grundlage der Verfügbarkeit von Datenpunkten zugeordnet, die zum Berechnen der Bewertung erforderlich sind.
- Anhand der Zuverlässigkeitsstufe können Sie die Zuverlässigkeit der von Azure Migrate bereitgestellten Größenempfehlungen besser einschätzen.
- Die Zuverlässigkeitsstufe gilt nicht für Bewertungen vom Typ *Wie lokal*.
- Die Serverbewertung benötigt für die leistungsbasierte Dimensionierung die folgenden Informationen:
    - Die Nutzungsdaten für CPU und VM-Arbeitsspeicher
    - IOPS- und Durchsatzdaten für jeden an den virtuellen Computer angefügten Datenträger
    - Informationen zur Netzwerk-E/A für die leistungsbasierte Größenanpassung für jeden Netzwerkadapter, der an einen virtuellen Computer angefügt ist

   Steht eine dieser Nutzungsangaben in vCenter Server nicht zur Verfügung, ist die Größenempfehlung unter Umständen nicht zuverlässig.

Die Zuverlässigkeitsstufe für die Bewertung ist abhängig davon, wie viele Datenpunkte verfügbar sind (in Prozent).

   **Verfügbarkeit von Datenpunkten** | **Zuverlässigkeitsstufe**
   --- | ---
   0–20 % | 1 Stern
   21–40 % | 2 Sterne
   41–60 % | 3 Sterne
   61–80 % | 4 Sterne
   81–100 % | 5 Sterne

> [!NOTE]
> Bewertungen von Servern, die über eine CSV-Datei in Azure Migrate importiert wurden, werden keine Zuverlässigkeitsstufen zugewiesen. 

### <a name="low-confidence-ratings"></a>Niedrige Zuverlässigkeitsstufen

Einige mögliche Gründe für eine niedrige Zuverlässigkeitsstufe einer Bewertung:

- Sie haben für den Zeitraum, für den Sie die Bewertung erstellen, kein Profil der Umgebung erstellt. Wenn Sie z. B. die Bewertung mit einer auf 1 Tag festgelegten Leistungsdauer erstellen, müssen Sie bis mindestens einen Tag nach dem Start der Ermittlung warten, bis alle Datenpunkte gesammelt sind.
- Einige virtuelle Computer wurden während des Zeitraums, für den die Bewertung berechnet wird, heruntergefahren. Wenn einige VMs für eine gewisse Zeit heruntergefahren wurden, kann die Serverbewertung für diesen Zeitraum keine Leistungsdaten sammeln.
- Einige virtuelle Computer wurden während des Zeitraums erstellt, für den die Bewertung berechnet wird. Wenn Sie beispielsweise eine Bewertung für den Leistungsverlauf des letzten Monats erstellen, aber einige VMs erst vor einer Woche in der Umgebung erstellt wurden, dann ist der Leistungsverlauf der neuen VMs nicht für die gesamte Dauer abrufbar.

> [!NOTE]
> Bei einer Zuverlässigkeitsstufe von weniger als fünf Sternen wird empfohlen, mindestens einen Tag zu warten, damit die Appliance ein Profil der Umgebung erstellen kann. Führen Sie dann eine Neuberechnung der Bewertung durch. Wenn Sie dies nicht tun, ist die leistungsbasierte Dimensionierung möglicherweise nicht zuverlässig. In diesem Fall wird empfohlen, die Bewertung auf die Größenanpassung „Wie lokal“ zu ändern.

## <a name="monthly-cost-estimation"></a>Schätzung der monatlichen Kosten

Nachdem die Größenempfehlungen ermittelt wurden, berechnet Azure Migrate die Compute- und Speicherkosten nach der Migration.

- **Computekosten**: Azure Migrate berechnet anhand der empfohlenen Azure-VM-Größe mithilfe der Abrechnungs-API die monatlichen Kosten für den virtuellen Computer.
    - Bei der Berechnung werden Betriebssystem, Software Assurance, reservierte Instanzen, VM-Betriebszeit, Standort und Währungseinstellungen berücksichtigt.
    - Die Kosten aller Computer werden zusammengefasst, um die monatlichen Gesamtcomputekosten zu berechnen.
- **Speicherkosten**: Die monatlichen Speicherkosten für einen Computer werden berechnet, indem die monatlichen Kosten aller an den Computer angefügten Datenträger wie folgt zusammengefasst werden:
    - Die Serverbewertung berechnet die gesamten monatlichen Speicherkosten durch Aggregieren der Speicherkosten aller Computer.
    - Derzeit werden bei der Berechnung keine Angebote berücksichtigt, die in den Bewertungseinstellungen angegeben sind.

Kosten werden in der Währung angezeigt, die in den Bewertungseinstellungen festgelegt ist.


## <a name="next-steps"></a>Nächste Schritte

Erstellen Sie eine Bewertung für [VMware-VMs](tutorial-assess-vmware.md) oder [Hyper-V-VMs](tutorial-assess-hyper-v.md).
