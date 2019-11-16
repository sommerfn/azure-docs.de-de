---
title: include file
description: include file
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/15/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 45168c23964c735a4bd51ba11c2340d4ff95fed4
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/13/2019
ms.locfileid: "74012412"
---
Azure Managed Disks (verwaltete Azure-Datenträger) stellt derzeit vier Datenträgertypen bereit, wobei jeder Typ auf bestimmte Kundenszenarien ausgerichtet ist.

## <a name="disk-comparison"></a>Datenträgervergleich

Die folgende Tabelle enthält eine Gegenüberstellung von Ultra-Datenträgern, SSD Premium-, SSD Standard- und HDD Standard-Laufwerken für verwaltete Datenträger, um Ihnen die Entscheidung zu erleichtern.

|   | Ultra-Datenträger   | SSD Premium   | SSD Standard   | HDD Standard   |
|---------|---------|---------|---------|---------|
|Datenträgertyp   |SSD   |SSD   |SSD   |Festplattenlaufwerk   |
|Szenario   |E/A-intensive Workloads wie [SAP HANA](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md), führende Datenbanken (z. B. SQL, Oracle) und andere Workloads mit vielen Transaktionen.   |Produktionsworkloads und leistungsabhängige Workloads   |Webserver, wenig genutzte Unternehmensanwendungen und Dev/Test   |Sicherung, nicht kritisch, sporadischer Zugriff   |
|Maximale Datenträgergröße   |65.536 Gibibyte (GiB)    |32767 GiB    |32767 GiB   |32767 GiB   |
|Max. Durchsatz   |2\.000 MiB/s    |900 MiB/s   |750 MiB/s   |500 MiB/s   |
|Max. IOPS   |160.000    |20.000   |6\.000   |2\.000   |

## <a name="ultra-disk"></a>Ultra-Datenträger

Azure Ultra-Datenträger bieten hohen Durchsatz, hohe IOPS und konsistenten Datenträgerspeicher mit niedrigen Wartezeiten für Azure IaaS-VMs. Zu den weiteren Vorteilen von Ultra-Datenträgern gehört die Möglichkeit, die Datenträgerleistung dynamisch in Abstimmung mit Ihren Workloads ändern zu können, ohne Ihre virtuellen Computer (virtual machines, VMs) neu starten zu müssen. Ultra-Datenträger eignen sich für datenintensive Workloads wie SAP HANA, führende Datenbanksysteme und Workloads mit vielen Transaktionen. Ultra-Datenträger können nur als Datenträger für Daten verwendet werden. Als Betriebssystemdatenträger empfehlen wir, SSD Premium-Datenträger zu verwenden.

### <a name="performance"></a>Leistung

Wenn Sie einen Ultra-Datenträger bereitstellen, können Sie die Kapazität und die Leistung des Datenträgers unabhängig voneinander konfigurieren. Ultra-Datenträger sind in mehreren festen Größen von 4 GiB bis 64 TiB erhältlich und bieten ein flexibles Leistungskonfigurationsmodell, durch das Sie IOPS und Durchsatz unabhängig konfigurieren können.

Einige Hauptfunktionen von Ultra-Datenträgern sind:

- Datenträgerkapazität: Die Kapazität von Ultra-Datenträgern reicht von 4 GiB bis 64 TiB.
- Datenträger-IOPS: Ultra-Datenträger unterstützen IOPS-Limits von 300 IOPS/GiB bis hin zu maximal 160.000 IOPS pro Datenträger. Um die bereitgestellten IOPS-Werte zu erreichen, stellen Sie sicher, dass der IOPS-Wert für den ausgewählten Datenträger unter dem IOPS-Limit für den virtuellen Computer liegt. Der kleinste IOPS-Wert pro Datenträger beträgt 2 IOPS/GiB mit einem Gesamtmindestwert von 100 IOPS. Wenn Sie also beispielsweise über einen 4-GiB-Ultra-Datenträger verfügen, stehen Ihnen anstelle von acht IOPS mindestens 100 IOPS zur Verfügung.
- Datenträgerdurchsatz: Mit Ultra-Datenträgern beträgt das Durchsatzlimit für einen einzelnen Datenträger 256 KiB/s für jeden bereitgestellten IOPS-Wert bis zu maximal 2000 MB/s pro Datenträger (dabei ist 1 MB/s = 10^6 Byte pro Sekunde). Der Mindestdurchsatz pro Datenträger beträgt 4KiB/s für jeden bereitgestellten IOPS-Wert mit einem Gesamtmindestwert von 1 MB/s.
- Ultra-Datenträger unterstützen die Anpassung der Datenträgerleistungsattribute (IOPS und Durchsatz) zur Laufzeit, ohne den Datenträger vom virtuellen Computer zu trennen. Nachdem ein Vorgang zur Größenänderung der Datenträgerleistung auf einem Datenträger gestartet wurde, kann es bis zu einer Stunde dauern, bis die Änderung tatsächlich wirksam wird. Innerhalb von 24 Stunden kann die Leistung bis zu viermal angepasst werden. Es kann vorkommen, dass ein Leistungsanpassungsvorgang aufgrund von unzureichender Kapazität der Leistungsbandbreite nicht erfolgreich ist.

### <a name="disk-size"></a>Datenträgergröße

|Datenträgergröße (GiB)  |IOPS-Limit  |Durchsatzlimit (MB/s)  |
|---------|---------|---------|
|4     |1\.200         |300         |
|8     |2\.400         |600         |
|16     |4\.800         |1\.200         |
|32     |9\.600         |2\.000         |
|64     |19.200         |2\.000         |
|128     |38.400         |2\.000         |
|256     |76.800         |2\.000         |
|512     |80.000         |2\.000         |
|1\.024–65.536 (Größen in diesem Bereich erhöhen sich in Schritten von 1 TiB)     |160.000         |2\.000         |

### <a name="ga-scope-and-limitations"></a>Umfang und Einschränkungen für allgemeine Verfügbarkeit

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](managed-disks-ultra-disks-GA-scope-and-limitations.md)]