---
title: 'SAP HANA: Speicherkonfigurationen für virtuelle Azure-Computer | Microsoft-Dokumentation'
description: Speicherempfehlungen für virtuelle Computer mit SAP HANA-Bereitstellung.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/25/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1faf6e4c9124d494507a124013d5fd8588f4b41b
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72934919"
---
# <a name="sap-hana-azure-virtual-machine-storage-configurations"></a>SAP HANA: Speicherkonfigurationen für virtuelle Azure-Computer

Azure bietet verschiedene geeignete Speichertypen für virtuelle Azure-Computer mit SAP HANA. Für **SAP HANA-zertifizierte Azure-Speichertypen** kommen unter anderem folgende Azure-Speichertypen in Frage: 

- Azure SSD Premium  
- [Ultra-Datenträger](https://docs.microsoft.com/azure/virtual-machines/linux/disks-enable-ultra-ssd)
- [Azure NetApp Files](https://azure.microsoft.com/services/netapp/) 

Weitere Informationen zu diesen Datenträgertypen finden Sie im Artikel [Auswählen eines Datenträgertyps](https://docs.microsoft.com/azure/virtual-machines/linux/disks-types).

Azure bietet zwei Bereitstellungsmethoden für virtuelle Festplatten (VHDs) in Azure Storage Standard und Premium. Wenn das allgemeine Szenario dies zulässt, sollten Sie Bereitstellungen mit [verwalteten Azure-Datenträgern](https://azure.microsoft.com/services/managed-disks/) verwenden. 

Eine Liste der Speichertypen und deren Vereinbarungen zum Servicelevel für IOPS- und Speicherdurchsatz finden Sie in der [Azure-Dokumentation für verwaltete Datenträger](https://azure.microsoft.com/pricing/details/managed-disks/).

Die minimalen für SAP HANA zertifizierten Bedingungen für die verschiedenen Speichertypen sind: 

- Azure SSD Premium: „/hana/log“ muss über die [Schreibbeschleunigung](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) von Azure zwischengespeichert werden. Das Volume „/hana/data“ kann ohne Azure-Schreibbeschleunigung auf SSD Premium oder auf Ultra-Datenträger platziert werden.
- Azure Ultra-Datenträger mindestens für das Volume „/hana/log“. Das Volume „/hana/data“ kann entweder ohne Azure-Schreibbeschleunigung auf SSD Premium oder für schnellere Neustartzeiten auf Ultra-Datenträger platziert werden.
- **NFS v4.1**-Volumes basierend auf Azure NetApp Files für „/hana/log“ **und** „/hana/data“ Das Volume von „/hana/shared“ kann das Protokoll NFS v3 oder NFS v4.1 verwenden. Das Protokoll NFS v4.1 ist für die Volumes „/hana/data“ und „/hana/log“ obligatorisch.

Einige Speichertypen können kombiniert werden. Es ist z. B. möglich, „/hana/data“ unter Storage Premium zu speichern, und „/hana/log“ kann unter Disk Storage Ultra gespeichert werden, um die erforderliche geringe Latenz zu erzielen. Wir raten aber davon ab, NFS-Volumes beispielsweise für „/hana/data“ zu mischen und einen der anderen zertifizierten Speichertypen für „/hana/log“ zu verwenden.

In der lokalen Umgebung mussten Sie sich nur selten Gedanken über die E/A-Subsysteme und deren Funktionen machen. Das lag daran, dass der Hersteller des Geräts sicherstellen musste, dass die Mindestspeicheranforderungen für SAP HANA erfüllt sind. Da Sie die Azure-Infrastruktur selbst erstellen, müssen Sie mit einigen dieser Anforderungen vertraut sein. Aufgrund der geforderten Merkmale für den Mindestdurchsatz ist Folgendes erforderlich:

- Ermöglichen von Lese-/Schreibvorgängen für **/hana/log** mit 250 MB/s bei E/A-Größen von 1 MB
- Aktivieren der Leseaktivität mit mindestens 400 MB/s in **/hana/data** bei E/A-Größen von 16 MB und 64 MB
- Aktivieren der Schreibaktivität mit mindestens 250 MB/s in **/hana/data** bei E/A-Größen von 16 MB und 64 MB

Da für DBMS-Systeme (und somit auch für SAP HANA) eine geringe Speicherlatenz wichtig ist, behalten sie Daten im Arbeitsspeicher. Der kritische Pfad beim Speichern sind in der Regel die Schreibvorgänge in das Transaktionsprotokoll der DBMS-Systeme. Aber auch Vorgänge wie das Schreiben von Sicherungspunkten oder das Laden von Daten in den Arbeitsspeicher nach der Wiederherstellung nach einem Systemabsturz können wichtig sein. Daher ist für die Volumes **/hana/data** und **/hana/log** die Verwendung von Azure Premium-Datenträgern **zwingend** erforderlich. Um den minimalen Durchsatz von **/hana/log** und **/hana/data** zu erreichen, wie er von SAP gewünscht ist, müssen Sie mithilfe von MDADM oder LVM über mehrere Azure Storage Premium-Datenträger ein RAID 0-Volume erstellen und die RAID-Volumes als die Volumes **/hana/data** und **/hana/log** verwenden. 

**Empfehlung: Als Stripegrößen für RAID 0 wird Folgendes empfohlen:**

- 64 KB oder 128 KB für **/hana/data**
- 32 KB für **/hana/log**

> [!NOTE]
> Sie müssen keine Redundanzebenen mit RAID-Volumes konfigurieren, da Azure Storage Premium und Standard drei Images einer virtuellen Festplatte beibehalten. Die Nutzung eines RAID-Volumes dient nur dazu, Volumes zu konfigurieren, die genügend E/A-Durchsatz bieten.

Das Kumulieren einer Reihe von Azure-VHDs unter einem RAID-Volume ist für den IOPS- und Speicherdurchsatz kumulativ. Wenn Sie also ein RAID 0-Volume über drei Azure Storage Premium-P30-Datenträgern platzieren, erhalten Sie den dreifachen IOPS- und den dreifachen Speicherdurchsatz eines einzelnen Azure Storage Premium-P30-Datenträgers.

Darüber hinaus sollten Sie den E/A-Gesamtdurchsatz eines virtuellen Computers beachten, wenn Sie die Größe festlegen oder sich für einen virtuellen Computer entscheiden. Der VM-Speicherdurchsatz im Allgemeinen ist im Artikel [Arbeitsspeicheroptimierte Größen virtueller Computer](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory) beschrieben.

## <a name="linux-io-scheduler-mode"></a>E/A-Scheduler-Modus für Linux
Linux verfügt über mehrere verschiedene E/A-Scheduling-Modi. Linux-Anbieter und SAP empfehlen im Allgemeinen, den E/A-Scheduler-Modus für Datenträgervolumes von **cfq** in **noop** zu ändern. Details finden Sie im [SAP-Hinweis 1984798](https://launchpad.support.sap.com/#/notes/1984787). 


## <a name="solutions-with-premium-storage-and-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Lösungen mit Storage Premium und Azure-Schreibbeschleunigung für virtuelle Azure-Computer der M-Serie
Die Azure-Schreibbeschleunigung ist eine Funktion, die ausschließlich für virtuelle Azure-Computer der M-Serie verfügbar ist. Der Name macht bereits deutlich, dass der Zweck der Funktion die Verbesserung der E/A-Wartezeit bei Schreibvorgängen für Azure Storage Premium ist. Für SAP HANA ist die Schreibbeschleunigung nur für das Volume **/hana/log** vorgesehen. **/hana/data** und **/hana/log** sind daher separate Volumes, und die Azure-Schreibbeschleunigung unterstützt nur das Volume **/hana/log**. 

> [!IMPORTANT]
> Bei Verwendung von Azure Storage Premium ist die Nutzung der [Schreibbeschleunigung](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) von Azure oder des Volumes „/hana/log“ obligatorisch. Die Schreibbeschleunigung ist nur für Storage Premium und VMs der Serien M und Mv2 verfügbar.

Bei den Cacheempfehlungen unten werden E/A-Merkmale für SAP HANA gemäß der folgenden Liste zugrunde gelegt:

- Es gibt kaum Workload durch Lesezugriffe auf die HANA-Datendateien. Ausnahmen betreffen umfangreiche E/As nach dem Neustart der HANA-Instanz oder wenn Daten in HANA geladen werden. Ein anderer Fall umfangreicherer Lese-E/As für Datendateien sind Sicherungen der HANA-Datenbank. Das hat zur Folge, dass Lesecaches meistens nicht sinnvoll sind, da in den meisten dieser Fälle alle Volumes mit Datendateien vollständig gelesen werden müssen.
- Das Schreiben in die Datendateien geschieht in Bursts auf der Grundlage der HANA-Sicherungspunkte und der HANA-Absturzwiederherstellung. Das Schreiben von Sicherungspunkten erfolgt asynchron und hält Benutzertransaktionen in keiner Weise auf. Beim Schreiben von Daten während der Wiederherstellung nach Abstürzen ist die Leistung ein kritischer Faktor, um schnell wieder zu einem reaktionsbereiten System zu kommen. Die Wiederherstellung nach Abstürzen dürfte allerdings eher eine Ausnahmesituation darstellen
- Es gibt kaum Lesevorgänge aus den HANA-Wiederholungsdateien. Ausnahmen bilden umfangreiche E/A-Vorgänge beim Ausführen von Sicherungen des Transaktionsprotokolls, der Wiederherstellung nach Abstürzen oder in der Neustartphase einer HANA-Instanz.  
- Der hauptsächliche Workload bei SAP HANA-Wiederholungsprotokolldateien besteht aus Schreibvorgängen. Abhängig von der Art des Workloads kann die Größe von E/As bis zu 4 KB herunter- oder in anderen Fällen bis zu 1 MB oder mehr heraufreichen. Wartezeiten beim Schreiben in das SAP HANA-Wiederholungsprotokoll sind ein kritischer Faktor für die Leistung.
- Alle Schreibvorgänge müssen zuverlässig und dauerhaft auf einem Datenträger gespeichert werden.

**Empfehlung: Aufgrund dieser E/A-Muster von SAP HANA sollte das Caching für die verschiedenen Volumes unter Verwendung von Azure Storage Premium wie folgt festgelegt werden:**

- **/hana/data** – kein Caching
- **/hana/log** – Kein Caching: Ausnahme für M- und Mv2-Serie, für die die Schreibbeschleunigung ohne Zwischenspeicherung von Lesevorgängen aktiviert ist. 
- **/hana/shared** – Read-Caching

### <a name="production-recommended-storage-solution"></a>Für die Produktion empfohlene Speicherlösung

> [!IMPORTANT]
> Die SAP HANA-Zertifizierung für virtuelle Computer der Azure M-Serie gilt ausschließlich mit der Azure-Schreibbeschleunigung für das Volume **/hana/log**. Folglich müssen SAP HANA-Bereitstellungen in Produktionsszenarien auf virtuellen Computern der Azure M-Serie für das Volume **/hana/log** mit der Azure-Schreibbeschleunigung konfiguriert werden.  

> [!NOTE]
> Überprüfen Sie für Produktionsszenarien in der [SAP-Dokumentation für IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html), ob ein bestimmter VM-Typ von SAP für SAP HANA unterstützt wird.



**Empfehlung: Für Produktionsszenarien werden folgende Konfigurationen empfohlen:**

| VM-SKU | RAM | Maximal VM-E/A<br /> Throughput | /hana/data | /hana/log | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GB | 1000 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 GiB | 1000 MB/s | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 2 x P40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P40 |
| M208s_v2 | 2\.850 GiB | 1000 MB/s | 4 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P40 |
| M208ms_v2 | 5700 GiB | 1000 MB/s | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416s_v2 | 5700 GiB | 2000 MB/s | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416ms_v2 | 11400 GiB | 2000 MB/s | 8 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P50 |

Virtuelle Computer vom Typ „M416xx_v2“ wurden von Microsoft noch nicht öffentlich verfügbar gemacht. Überprüfen Sie, ob der Speicherdurchsatz für die verschiedenen vorgeschlagenen Volumes für die Workload ausreicht, die Sie ausführen möchten. Wenn die Workload größere Volumes für **/hana/data** und **/hana/log** erfordert, müssen Sie die Anzahl der Azure Storage Premium-VHDs erhöhen. Wenn Sie ein Volume mit mehr VHDs ausstatten als in der Liste angegeben, erhöht sich der IOPS- und E/A-Durchsatz innerhalb der Grenzen des Azure-VM-Typs.

Die Azure-Schreibbeschleunigung funktioniert nur in Verbindung mit [verwalteten Azure-Datenträgern](https://azure.microsoft.com/services/managed-disks/). Daher müssen zumindest die Azure Storage Premium-Datenträger, die das Volume **/hana/log** bilden, als verwaltete Datenträger bereitgestellt werden.

Die Anzahl von Azure Storage Premium-VHDs pro VM, die von der Azure-Schreibbeschleunigung unterstützt werden können, ist begrenzt. Die aktuellen Limits lauten wie folgt:

- 16 VHDs für eine VM der Typen M128xx und M416xx
- 8 VHDs für eine VM der Typen M64xx und M208xx
- 4 VHDs für eine M32xx-VM

Ausführlichere Anweisungen zum Aktivieren der Azure-Schreibbeschleunigung finden Sie im Artikel [Schreibbeschleunigung](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Details und Einschränkungen für die Azure-Schreibbeschleunigung finden Sie in der gleichen Dokumentation.

**Empfehlung: Verwenden Sie die Schreibbeschleunigung für Datenträger, die das Volume „/hana/log“ bilden.**


### <a name="cost-conscious-azure-storage-configuration"></a>Kostenbewusste Azure Storage-Konfiguration
In der folgenden Tabelle ist eine Konfiguration mit VM-Typen aufgeführt, die Kunden ebenfalls verwenden, um SAP HANA auf Azure-VMs zu hosten. Einige VM-Typen erfüllen unter Umständen nicht alle Mindestspeicherkriterien für SAP HANA oder werden von SAP nicht offiziell für SAP HANA unterstützt. Aber bisher scheinen diese VMs für Nicht-Produktionsszenarien problemlos zu funktionieren. **/hana/data** und **/hana/log** werden zu einem einzelnen Volume zusammengefasst. Dadurch kann sich der Einsatz der Azure-Schreibbeschleunigung zu einem limitierenden Faktor im IOPS-Bereich entwickeln.

> [!NOTE]
> Überprüfen Sie für Szenarien mit SAP-Unterstützung in der [SAP-Dokumentation für IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html), ob ein bestimmter VM-Typ von SAP für SAP HANA unterstützt wird.

> [!NOTE]
> Eine Änderung gegenüber früheren Empfehlungen für eine preisgünstige Lösung stellt der Wechsel von [Azure HDD Standard-Laufwerken](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-hdd) zu [Azure SSD Standard-Datenträgern](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-ssd) mit höherer Leistung und Zuverlässigkeit dar.


| VM-SKU | RAM | Maximal VM-E/A<br /> Throughput | „/hana/data“ und „/hana/log“<br /> Striping mit LVM oder MDADM | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 GiB | 768 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E16v3 | 128 GB | 384 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E32v3 | 256 GiB | 768 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| E64v3 | 432 GiB | 1\.200 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 × E30 |
| GS5 | 448 GiB | 2\.000 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 × E30 |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M64ls | 512 GB | 1\.000 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 |1 × E30 |
| M64s | 1\.000 GiB | 1\.000 MB/s | 2 x P30 | 1 × E30 | 1 x E6 | 1 x E6 |2 × E30 |
| M64ms | 1\.750 GiB | 1\.000 MB/s | 3 x P30 | 1 × E30 | 1 x E6 | 1 x E6 | 3 x E30 |
| M128s | 2\.000 GiB | 2\.000 MB/s |3 x P30 | 1 × E30 | 1 x E10 | 1 x E6 | 2 × E40 |
| M128ms | 3\.800 GiB | 2\.000 MB/s | 5 x P30 | 1 × E30 | 1 x E10 | 1 x E6 | 2 × E50 |
| M208s_v2 | 2\.850 GiB | 1\.000 MB/s | 4 x P30 | 1 × E30 | 1 x E10 | 1 x E6 |  3 × E40 |
| M208ms_v2 | 5\.700 GiB | 1\.000 MB/s | 4 x P40 | 1 × E30 | 1 x E10 | 1 x E6 |  4 × E40 |
| M416s_v2 | 5\.700 GiB | 2\.000 MB/s | 4 x P40 | 1 × E30 | 1 x E10 | 1 x E6 |  4 × E40 |
| M416ms_v2 | 11400 GiB | 2\.000 MB/s | 8 x P40 | 1 × E30 | 1 x E10 | 1 x E6 |  4 × E50 |


Virtuelle Computer vom Typ „M416xx_v2“ wurden von Microsoft noch nicht öffentlich verfügbar gemacht. Die Datenträger, die für die kleineren Typen virtueller Computer mit 3 x P20 empfohlen werden, haben Übergröße im Vergleich zu den Speicherplatzempfehlungen, die im [SAP-Whitepaper zu TDI Speicher](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) angegeben sind. Die in der Tabelle gezeigte Wahl wurde getroffen, um ausreichend Datenträgerdurchsatz für SAP HANA bereitzustellen. Sie können problemlos Änderungen am Volume **/hana/backup** vornehmen, dessen Größe so festgelegt wurde, dass es Sicherungen aufnehmen kann, die dem Zweifachen der Arbeitsspeichergröße entsprechen.   
Überprüfen Sie, ob der Speicherdurchsatz für die verschiedenen vorgeschlagenen Volumes für die Workload ausreicht, die Sie ausführen möchten. Wenn die Workload größere Volumes für **/hana/data** und **/hana/log** erfordert, müssen Sie die Anzahl der Azure Storage Premium-VHDs erhöhen. Wenn Sie ein Volume mit mehr VHDs ausstatten als in der Liste angegeben, erhöht sich der IOPS- und E/A-Durchsatz innerhalb der Grenzen des Azure-VM-Typs. 

> [!NOTE]
> Für die oben angegebenen Konfigurationen bietet die [Vereinbarung zum Servicelevel für einzelne virtuelle Azure-Computer](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) KEINE Vorteile, da Azure Storage Premium und Azure Storage Standard gemeinsam verwendet werden. Die Auswahl wurde jedoch getroffen, um die Kosten zu optimieren. Sie müssten für alle Datenträger, die oben als Azure Storage Standard (Sxx) angegeben sind, Storage Premium auswählen, um die Konfiguration des virtuellen Computers mit der Vereinbarung zum Servicelevel für einen einzelnen virtuellen Azure-Computer kompatibel zu machen.


> [!NOTE]
> Die aufgeführten Empfehlungen für die Datenträgerkonfiguration orientieren sich an Mindestanforderungen, die SAP gegenüber seinen Infrastrukturanbietern formuliert. In realen Kundenbereitstellungen und Workloadszenarien traten Situationen ein, in denen diese Empfehlungen noch keine ausreichenden Funktionen bereitstellen konnten. Hierbei konnte es sich um Situationen handeln, in denen Kunden ein schnelleres erneutes Laden der Daten nach einem Neustart von HANA benötigten, oder wo Sicherungskonfigurationen eine höhere Bandbreite zum Speicher benötigten. Ein anderer Fall war **/hana/log**, wobei 5.000 IOPS für die spezifische Workload nicht ausreichend waren. Betrachten Sie also diese Empfehlungen als Ausgangspunkt, und nehmen Sie Anpassungen auf Grundlage der Anforderungen der Workload vor.
>  

## <a name="azure-ultra-disk-storage-configuration-for-sap-hana"></a>Azure Ultra-Datenträgerspeicherkonfiguration für SAP HANA
Microsoft führt derzeit einen neuen Azure-Speichertyp namens [Azure Ultra-Datenträger](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#ultra-disk) ein. Der wesentliche Unterschied zwischen dem bisher angebotenen Azure-Speicher und einem Ultra-Datenträger besteht darin, dass die Datenträgermerkmale nicht mehr an die Größe des Datenträgers gebunden sind. Als Kunde können Sie diese Merkmale für einen Ultra-Datenträger definieren:

- Größe eines Datenträgers zwischen 4 GiB und 65.536 GiB
- IOPS-Bereich zwischen 100 IOPS und 160.000 IOPS (Höchstwert abhängig von VM-Typen)
- Speicherdurchsatz zwischen 300 MB/s und 2.000 MB/s

Mit einem Ultra-Datenträger können Sie einen einzelnen Datenträger definieren, der Ihre Anforderungen hinsichtlich Größe, IOPS und Datenträgerdurchsatz erfüllt. Sie müssen also keine Manager für logische Volumes wie LVM oder MDADM zusätzlich zu Azure Storage Premium mehr verwenden, um Volumes zu erstellen, die Ihre Anforderungen an IOPS und Durchsatz erfüllen. Sie können eine Konfigurationsmischung aus Ultra-Datenträgern und Storage Premium ausführen. Auf diese Weise können Sie die Verwendung von Ultra-Datenträgern auf die leistungskritischen Volumes „/hana/data“ und „/hana/log“ beschränken und die anderen Volumes mit Azure Storage Premium nutzen.

Ein weiterer Vorteil eines Ultra-Datenträgers kann auch die bessere Leselatenz im Vergleich mit Storage Premium sein. Eine geringere Leselatenz kann auch zu Vorteilen führen, wenn Sie die HANA-Startdauern und den nachfolgenden Ladevorgang der Daten in den Arbeitsspeicher verkürzen möchten. Weitere Vorteile von Disk Storage Ultra können sich zudem ergeben, wenn von HANA Sicherungspunkte geschrieben werden. Da Storage Premium-Datenträger für „/hana/data“ normalerweise nicht per Schreibbeschleunigung zwischengespeichert werden, ist die Schreiblatenz für „/hana/data“ unter Storage Premium verglichen mit dem Ultra-Datenträger höher. Es ist zu erwarten, dass das Schreiben von Sicherungspunkten per Ultra-Datenträger auf dem Ultra-Datenträger eine bessere Leistung aufweist.

> [!IMPORTANT]
> Ultra-Datenträger sind noch nicht in allen Azure-Regionen vorhanden und unterstützen noch nicht alle unten aufgeführten VM-Typen. Ausführliche Informationen dazu, wo Ultra-Datenträger verfügbar sind und welche VM-Familien unterstützt werden, finden Sie im Artikel [Welche Datenträgertypen stehen in Azure zur Verfügung?](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#ultra-disk).

### <a name="production-recommended-storage-solution-with-pure-ultra-disk-configuration"></a>Für die Produktion empfohlene Speicherlösung mit reiner Ultra-Datenträgerkonfiguration
Bei dieser Konfiguration bleiben die Volumes „/hana/data“ und „/hana/log“ getrennt. Die vorgeschlagenen Werte werden von den KPIs abgeleitet, die von SAP zur Zertifizierung von VM-Typen für SAP HANA und Speicherkonfigurationen verwendet werden (gemäß Empfehlung im [SAP TDI Storage-Whitepaper](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html)).

| VM-SKU | RAM | Maximal VM-E/A<br /> Throughput | Volume „/hana/data“ | E/A-Durchsatz für „/hana/data“ | IOPS für „/hana/data“ | Volume „/hana/log“ | E/A-Durchsatz für „/hana/log“ | IOPS für „/hana/log“ |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| E64s_v3 | 432 GiB | 1\.200 MB/s | 600 GB | 700 MBit/s | 7\.500 | 512 GB | 500 MBit/s  | 2\.000 |
| M32ts | 192 GiB | 500 MB/s | 250 GB | 400 MBit/s | 7\.500 | 256 GB | 250 MBit/s  | 2\.000 |
| M32ls | 256 GiB | 500 MB/s | 300 GB | 400 MBit/s | 7\.500 | 256 GB | 250 MBit/s  | 2\.000 |
| M64ls | 512 GB | 1\.000 MB/s | 600 GB | 600 MBit/s | 7\.500 | 512 GB | 400 MBit/s  | 2\.500 |
| M64s | 1\.000 GiB | 1\.000 MB/s |  1\.200 GB | 600 MBit/s | 7\.500 | 512 GB | 400 MBit/s  | 2\.500 |
| M64ms | 1\.750 GiB | 1\.000 MB/s | 2\.100 GB | 600 MBit/s | 7\.500 | 512 GB | 400 MBit/s  | 2\.500 |
| M128s | 2\.000 GiB | 2\.000 MB/s |2\.400 GB | 1\.200 MBit/s |9\.000 | 512 GB | 800 MBit/s  | 3,000 | 
| M128ms | 3\.800 GiB | 2\.000 MB/s | 4\.800 GB | 1\.200 MBit/s |9\.000 | 512 GB | 800 MBit/s  | 3,000 | 
| M208s_v2 | 2\.850 GiB | 1\.000 MB/s | 3\.500 GB | 1\.000 MBit/s | 9\.000 | 512 GB | 400 MBit/s  | 2\.500 | 
| M208ms_v2 | 5\.700 GiB | 1\.000 MB/s | 7\.200 GB | 1\.000 MBit/s | 9\.000 | 512 GB | 400 MBit/s  | 2\.500 | 
| M416s_v2 | 5\.700 GiB | 2\.000 MB/s | 7\.200 GB | 1\.500 MBit/s | 9\.000 | 512 GB | 800 MBit/s  | 3,000 | 
| M416ms_v2 | 11.400 GiB | 2\.000 MB/s | 14.400 GB | 1\.500 MBit/s | 9\.000 | 512 GB | 800 MBit/s  | 3,000 |   

Virtuelle Computer vom Typ „M416xx_v2“ wurden von Microsoft noch nicht öffentlich verfügbar gemacht. Die angegebenen Werte sind lediglich als Ausgangspunkt gedacht und müssen auf die tatsächlichen Anforderungen abgestimmt werden. Der Vorteil eines Azure Ultra-Datenträgers besteht darin, dass die Werte für IOPS und Durchsatz angepasst werden können, ohne den virtuellen Computer herunterzufahren oder die im System verarbeitete Workload anzuhalten.   

> [!NOTE]
> Bisher sind noch keine Speichermomentaufnahmen mit Ultra-Datenträgerspeicher verfügbar. Dadurch wird die Verwendung von VM-Momentaufnahmen mit Azure Backup-Diensten blockiert.

### <a name="production-recommended-storage-solution-with-pure-ultra-disk-configuration"></a>Für die Produktion empfohlene Speicherlösung mit reiner Ultra-Datenträgerkonfiguration
Bei dieser Konfiguration befinden sich die Volumes „/hana/data“ und „/hana/log“ auf demselben Datenträger. Die vorgeschlagenen Werte werden von den KPIs abgeleitet, die von SAP zur Zertifizierung von VM-Typen für SAP HANA und Speicherkonfigurationen verwendet werden (gemäß Empfehlung im [SAP TDI Storage-Whitepaper](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html)).

| VM-SKU | RAM | Maximal VM-E/A<br /> Throughput | Volume für „/hana/data“ und „/log“ | E/A-Durchsatz von „/hana/data“ und „/log“ | IOPS von „/hana/data“ und „/log“ |
| --- | --- | --- | --- | --- | --- |
| E64s_v3 | 432 GiB | 1\.200 MB/s | 1\.200 GB | 1\.200 MBit/s | 9\.500 | 
| M32ts | 192 GiB | 500 MB/s | 512 GB | 400 MBit/s | 9\.500 | 
| M32ls | 256 GiB | 500 MB/s | 600 GB | 400 MBit/s | 9\.500 | 
| M64ls | 512 GB | 1\.000 MB/s | 1\.100 GB | 900 MBit/s | 10.000 | 
| M64s | 1\.000 GiB | 1\.000 MB/s |  1\.700 GB | 900 MBit/s | 10.000 | 
| M64ms | 1\.750 GiB | 1\.000 MB/s | 2\.600 GB | 900 MBit/s | 10.000 | 
| M128s | 2\.000 GiB | 2\.000 MB/s |2\.900 GB | 1\.800 MBit/s |12.000 | 
| M128ms | 3\.800 GiB | 2\.000 MB/s | 5\.300 GB | 1\.800 MBit/s |12.000 |  
| M208s_v2 | 2\.850 GiB | 1\.000 MB/s | 4\.000 GB | 900 MBit/s | 10.000 |  
| M208ms_v2 | 5\.700 GiB | 1\.000 MB/s | 7\.700 GB | 900 MBit/s | 10.000 | 
| M416s_v2 | 5\.700 GiB | 2\.000 MB/s | 7\.700 GB | 1\.800 MBit/s | 12.000 |  
| M416ms_v2 | 11.400 GiB | 2\.000 MB/s | 15.000 GB | 1\.800 MBit/s | 12.000 |    

Virtuelle Computer vom Typ „M416xx_v2“ wurden von Microsoft noch nicht öffentlich verfügbar gemacht. Die angegebenen Werte sind lediglich als Ausgangspunkt gedacht und müssen auf die tatsächlichen Anforderungen abgestimmt werden. Der Vorteil eines Azure Ultra-Datenträgers besteht darin, dass die Werte für IOPS und Durchsatz angepasst werden können, ohne den virtuellen Computer herunterzufahren oder die im System verarbeitete Workload anzuhalten.  

## <a name="nfs-v41-volumes-on-azure-netapp-files"></a>NFS v4.1-Volumes unter Azure NetApp Files
Azure NetApp Files verfügt über native NFS-Freigaben, die für die Volumes „/hana/shared“, „/hana/data“ und „/hana/log“ verwendet werden können. Zur Nutzung von ANF-basierten NFS-Freigaben für die Volumes „/hana/data“ und „/hana/log“ wird das v4.1 NFS-Protokoll benötigt. Das NFS-Protokoll v3 wird nicht für die Nutzung der Volumes „/hana/data“ und „/hana/log“ unterstützt, wenn die Freigaben auf ANF basieren. 

> [!IMPORTANT]
> Das NFS v3-Protokoll, das für Azure NetApp Files implementiert ist, wird für die Verwendung für „/hana/data“ und „/hana/log“ nicht unterstützt. Die Verwendung von NFS 4.1 ist für die Volumes „/hana/data“ und „/hana/log“ aus funktionaler Sicht obligatorisch. Während für das Volume „/hana/shared“ das Protokoll NFS v3 oder NFS v4.1 aus funktionaler Sicht verwendet werden kann.

### <a name="important-considerations"></a>Wichtige Hinweise
Wenn Sie Azure NetApp Files für die Hochverfügbarkeitsarchitektur von SAP NetWeaver und SAP HANA in Betracht ziehen, beziehen Sie die folgenden wichtigen Überlegungen mit ein:

- Die Mindestgröße eines Kapazitätspools beträgt 4 TiB.  
- Die Mindestvolumegröße ist 100 GiB.
- Azure NetApp Files und alle virtuellen Computer, auf denen Azure NetApp Files-Volumes eingebunden werden sollen, müssen sich im selben virtuellen Azure-Netzwerk oder in [über Peering gekoppelten virtuellen Netzwerken](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) in derselben Region befinden.  
- Das ausgewählte virtuelle Netzwerk muss über ein an Azure NetApp Files delegiertes Subnetz verfügen.
- Der Durchsatz eines Azure NetApp-Volumes ist eine Funktion des Volumekontingents und der Dienstebene, wie in [Dienstebenen für Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) beschrieben. Stellen Sie bei der Größenanpassung der HANA Azure NetApp-Volumes sicher, dass der sich ergebende Durchsatz die HANA-Systemanforderungen erfüllt.  
- Azure NetApp Files bietet [Exportrichtlinien](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-configure-export-policy): Sie können die zulässigen Clients und den Zugriffstyp (Lesen und Schreiben, schreibgeschützt usw.) steuern. 
- Azure NetApp Files wertet derzeit noch keine Zonen aus. Das Azure NetApp Files-Feature wird bisher nicht in allen Verfügbarkeitszonen in einer Azure-Region bereitgestellt. Achten Sie auf mögliche Latenzauswirkungen in einigen Azure-Regionen.  
- Um eine niedrige Latenz zu erzielen, ist es wichtig, dass die virtuellen Computer in unmittelbarer Nähe des Azure NetApp-Speichers bereitgestellt werden. Bei SAP HANA-Workloads ist eine niedrige Latenz sehr wichtig. Arbeiten Sie mit Ihrem Microsoft-Vertreter zusammen, um sicherzustellen, dass die virtuellen Computer und die Azure NetApp Files-Volumes in unmittelbarer Nähe zueinander bereitgestellt werden.  
- Die Benutzer-ID für „<b>sid</b>adm“ und die Gruppen-ID für `sapsys` auf den virtuellen Computern müssen mit der Konfiguration in Azure NetApp Files übereinstimmen. 

> [!IMPORTANT]
> Bei SAP HANA-Workloads ist eine niedrige Latenz sehr wichtig. Arbeiten Sie mit Ihrem Microsoft-Vertreter zusammen, um sicherzustellen, dass die virtuellen Computer und die Azure NetApp Files-Volumes in unmittelbarer Nähe zueinander bereitgestellt werden.  

> [!IMPORTANT]
> Wenn die Benutzer-ID für „<b>sid</b>adm“ und die Gruppen-ID für `sapsys` auf den virtuellen Computern und in der Azure NetApp-Konfiguration nicht übereinstimmen, werden die Berechtigungen für Dateien auf Azure NetApp-Volumes, die auf virtuellen Computern bereitgestellt sind, als `nobody` angezeigt. Stellen Sie beim [Onboarding eines neuen Systems](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRxjSlHBUxkJBjmARn57skvdUQlJaV0ZBOE1PUkhOVk40WjZZQVJXRzI2RC4u) in Azure NetApp Files sicher, dass Sie die richtige Benutzer-ID für „<b>sid</b>adm“ und die Gruppen-ID für `sapsys` angeben.

### <a name="sizing-for-hana-database-on-azure-netapp-files"></a>Größenanpassung für eine HANA-Datenbank in Azure NetApp Files

Der Durchsatz eines Azure NetApp-Volumes ist eine Funktion der Volumegröße und der Dienstebene, wie in [Dienstebenen für Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) beschrieben. 

Beim Entwerfen der Infrastruktur für SAP in Azure müssen Sie einige Mindestanforderungen von SAP an den Speicher beachten, aus denen sich die Mindestdurchsatzeigenschaften ergeben:

- Lese-/Schreibaktivität mit 250 MB/s in „/hana/log“ bei einer E/A-Größe von 1 MB aktivieren  
- Leseaktivität mit mindestens 400 MB/s in „/hana/data“ für E/A-Größen von 16 MB und 64 MB aktivieren  
- Schreibaktivität mit mindestens 250 MB/s in „/hana/data“ für E/A-Größen von 16 MB und 64 MB aktivieren  

Die [Azure NetApp Files-Durchsatzlimits](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-service-levels) pro 1 TiB an Volumekontingent lauten:
- Storage Premium-Tarif: 64 MIB/s  
- Storage Ultra-Tarif: 128 MIB/s  

Um die SAP-Mindestanforderungen für den Durchsatz für Daten und Protokolle und die Richtlinien für `/hana/shared` zu erfüllen, werden folgende Größen empfohlen:

| Volume | Size<br /> Storage Premium-Tarif | Size<br /> Storage Ultra-Tarif | Unterstütztes NFS-Protokoll |
| --- | --- | --- |
| /hana/log/ | 4 TiB | 2 TiB | v4.1 |
| /hana/data | 6,3 TiB | 3,2 TiB | v4.1 |
| /hana/shared | Max. 512 GB, 1 x RAM pro 4 Workerknoten | Max. 512 GB, 1 x RAM pro 4 Workerknoten | v3 oder v4.1 |

Die SAP HANA-Konfiguration für das in diesem Artikel vorgestellte Layout mit Azure NetApp Files-Tarif „Storage Ultra“ sieht wie folgt aus:

| Volume | Size<br /> Storage Ultra-Tarif | Unterstütztes NFS-Protokoll |
| --- | --- |
| /hana/log/mnt00001 | 2 TiB | v4.1 |
| /hana/log/mnt00002 | 2 TiB | v4.1 |
| /hana/data/mnt00001 | 3,2 TiB | v4.1 |
| /hana/data/mnt00002 | 3,2 TiB | v4.1 |
| /hana/shared | 2 TiB | v3 oder v4.1 |

> [!NOTE]
> Die hier angegebenen Empfehlungen für die Azure NetApp Files-Größenanpassung zielen darauf ab, die Mindestanforderungen zu erfüllen, die SAP an seine Infrastrukturanbieter stellt. In realen Kundenbereitstellungen und Workloadszenarien sind sie möglicherweise nicht ausreichend. Verwenden Sie diese Empfehlungen also als Ausgangspunkt, und nehmen Sie Anpassungen auf Grundlage der Anforderungen der spezifischen Workload vor.  

> [!TIP]
> Sie können die Größe der Azure NetApp Files-Volumes dynamisch anpassen, ohne die Bereitstellung der Volumes aufheben (`unmount`) oder die virtuellen Computer oder SAP HANA beenden zu müssen. Damit kann Ihre Anwendung sowohl den erwarteten als auch unvorhergesehenen Durchsatzanforderungen flexibel gerecht werden.

Die Dokumentation zur Bereitstellung einer SAP HANA-Konfiguration für das horizontale Hochskalieren mit Standbyknoten über NFS v4.1-Volumes, die in ANF gehostet werden, ist in [Horizontale SAP HANA-Skalierung mit Standbyknoten auf Azure-VMs mit Azure NetApp Files auf SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse) veröffentlicht.


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter

- [Hochverfügbarkeit von SAP HANA für virtuelle Azure-Computer](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
