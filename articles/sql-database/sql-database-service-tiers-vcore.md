---
title: Übersicht über das V-Kern-Modell
description: Mit dem V-Kern-Kaufmodell können Sie Compute- und Speicherressourcen einzeln skalieren, eine Leistung wie in Ihrer lokalen Umgebung erzielen und den Preis optimieren.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
ms.date: 11/04/2019
ms.openlocfilehash: b9de02bf0836727ac88b78194641238621e87a79
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73821058"
---
# <a name="vcore-model-overview"></a>Übersicht über das V-Kern-Modell

Das V-Kern-Modell (virtueller Kern) bietet mehrere Vorteile:

- Höhere Limits für Compute, Arbeitsspeicher, E/A und Speicher
- Kontrolle über die Hardwaregeneration, um besser auf Compute- und Arbeitsspeicheranforderungen der Workload reagieren zu können
- Preisrabatte für [Azure-Hybridvorteil](sql-database-azure-hybrid-benefit.md) und [reservierte Instanzen (RI)](sql-database-reserved-capacity.md)
- Mehr Transparenz bei den Hardwaredetails für das Computing zur besseren Planung von Migrationsvorgängen von lokalen Bereitstellungen

## <a name="service-tiers"></a>Dienstebenen

Als Optionen für die Dienstebene stehen im V-Kern-Modell „Universell“, „Unternehmenskritisch“ und „Hyperscale“ zur Verfügung. Die Dienstebene definiert ganz allgemein Speicherarchitektur, Speicherplatz- und E/A-Limits sowie Optionen für die Geschäftskontinuität im Zusammenhang mit der Verfügbarkeit und Notfallwiederherstellung.

||**Allgemeiner Zweck**|**Unternehmenskritisch**|**Hyperscale**|
|---|---|---|---|
|Am besten geeignet für:|Die meisten geschäftlichen Workloads. Bietet budgetorientierte, ausgewogene und skalierbare Compute- und Speicheroptionen. |Bietet Geschäftsanwendungen die höchste Resilienz gegenüber Fehlern durch die Verwendung mehrerer isolierter Replikate sowie die höchste E/A-Leistung pro Datenbankreplikat.|Die meisten geschäftlichen Workloads mit hohen Anforderungen an skalierbaren Speicher und Leseskalierung.  Bietet eine höhere Ausfallsicherheit, da mehrere isolierte Datenbankreplikate konfiguriert werden können. |
|Storage|Verwendet Remotespeicher.<br/>**Einzeldatenbank und Pool für elastische Datenbanken: Bereitgestelltes Computing**:<br/>5 GB – 4 TB<br/>**Serverloses Computing**:<br/>5 GB bis 3 TB<br/>**Verwaltete Instanz**: 32 GB – 8 TB |Verwendet lokalen SSD-Speicher.<br/>**Einzeldatenbank und Pool für elastische Datenbanken: Bereitgestelltes Computing**:<br/>5 GB – 8 TB<br/>**Verwaltete Instanz**:<br/>32 GB – 4 TB |Flexible automatische Speichervergrößerung nach Bedarf. Unterstützt bis zu 100 TB Speicher. Verwendet lokalen SSD-Speicher für den lokalen Pufferpoolcache und den lokalen Datenspeicher. Verwendet Azure-Remotespeicher als endgültigen langfristigen Datenspeicher. |
|E/A-Durchsatz (ungefähr)|**Einzeldatenbank und Pool für elastische Datenbanken**: 500 IOPS pro V-Kern, bis zu 40.000 IOPS maximal.<br/>**Verwaltete Instanz**: Abhängig von der [Größe der Datei](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes).|5\.000 IOPS pro V-Kern, bis zu 320.000 IOPS maximal|Hyperscale ist eine mehrstufige Architektur mit Caching auf mehreren Ebenen. Die tatsächlichen IOPs hängen von der Workload ab.|
|Verfügbarkeit|Einzelnes Replikat, keine Replikate mit Leseskalierung|3 Replikate, 1 [Replikat, Leseskalierung](sql-database-read-scale-out.md),<br/>zonenredundante Hochverfügbarkeit (High Availability, HA)|Einzelnes Replikat mit Lese-/Schreibzugriff sowie bis zu vier [Replikate mit Leseskalierung](sql-database-read-scale-out.md)|
|Backups|[Georedundanter Speicher mit Lesezugriff (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md), sieben bis 35 Tage (standardmäßig sieben Tage)|[RA-GRS](../storage/common/storage-designing-ha-apps-with-ragrs.md), 7-35 Tage (standardmäßig 7 Tage)|Auf Momentaufnahmen basierende Sicherungen in Azure-Remotespeicher. Bei Wiederherstellungen werden diese Momentaufnahmen zur schnellen Wiederherstellung verwendet. Sicherungen werden sofort ausgeführt und haben keine Auswirkungen auf die E/A-Computeleistung. Wiederherstellungen sind schnell und nicht datenintensiv (dauern also nicht Stunden oder Tage, sondern nur Minuten).|
|In-Memory|Nicht unterstützt|Unterstützt|Nicht unterstützt|
|||


### <a name="choosing-a-service-tier"></a>Auswählen einer Dienstebene

Weitere Informationen zum Auswählen einer Dienstebene für die jeweilige Workload finden Sie in den folgenden Artikeln:

- [Wann sollte die Dienstebene „Universell“ ausgewählt werden?](sql-database-service-tier-general-purpose.md#when-to-choose-this-service-tier)
- [Wann sollte die Dienstebene „Unternehmenskritisch“ ausgewählt werden?](sql-database-service-tier-business-critical.md#when-to-choose-this-service-tier)
- [Wann sollte die Dienstebene „Hyperscale“ ausgewählt werden?](sql-database-service-tier-hyperscale.md#who-should-consider-the-hyperscale-service-tier)


## <a name="compute-tiers"></a>Computetarife

Als Optionen für den Computetarif stehen im V-Kern-Modell die bereitgestellten und serverlosen Computetarife zur Auswahl.


### <a name="provisioned-compute"></a>Bereitgestelltes Computing

Bereitgestellte Computetarife bieten eine bestimmte Menge an Computeressourcen, die unabhängig von der Workloadaktivität ständig bereitgestellt werden. Die Abrechnung erfolgt dann zu einem Festpreis pro Stunde für die Menge an bereitgestellten Computeressourcen.


### <a name="serverless-compute"></a>Serverloses Computing

Beim [serverlosen Computetarif](sql-database-serverless.md) erfolgt auch eine automatische Skalierung der Computeressourcen basierend auf den Workloadaktivitäten, und die Abrechnung erfolgt nach Menge der verbrauchten Computeressourcen pro Sekunde.



## <a name="hardware-generations"></a>Hardwaregenerationen

Zu den Optionen für die Hardwaregeneration im V-Kern-Modell gehören Gen4/Gen5, M-Serie (Vorschau) und Fsv2-Serie (Vorschau). Die Hardwaregeneration definiert im Allgemeinen die Compute- und Arbeitsspeicherlimits sowie weitere Eigenschaften, die sich auf die Leistung der Workload auswirken.

### <a name="gen4gen5"></a>Gen4/Gen5

- Gen4/Gen5-Hardware bietet ausgeglichene Compute- und Arbeitsspeicherressourcen und eignet sich für die meisten Datenbankworkloads, die keine erhöhten Anforderungen an Arbeitsspeicher, virtuelle Kerne oder schnellere Einzel-V-Kerne stellen, wie sie von der Fsv2-Serie oder der M-Serie bereitgestellt werden.

Informationen zu den Regionen, in denen Gen4/Gen5 verfügbar ist, finden Sie unter [Verfügbarkeit von Gen4/Gen5](#gen4gen5-1).

### <a name="fsv2-seriespreview"></a>Fsv2-Serie (Vorschau)

- Die Fsv2-Serie ist eine für Compute optimierte Hardwareoption, die eine niedrige CPU-Latenz und eine hohe Taktfrequenz für die meisten Workloads mit hohen CPU-Anforderungen bereitstellt.
- Abhängig von der Workload kann die Fsv2-Serie eine höhere CPU-Leistung pro virtuellem Kern erzielen als Gen5, und mit der Größe von 72 V-Kernen erzielen Sie mehr CPU-Leistung für weniger Kosten als mit 80 virtuellen Kernen bei Gen5. 
- Fsv2 bietet weniger Arbeitsspeicher und tempdb pro virtuellem Kern als andere Hardware, sodass für Workloads, für die diese Grenzwerte wichtig sind, stattdessen Gen5 oder die M-Serie in Erwägung gezogen werden sollte.  

Weitere Informationen zu Regionen, in denen die Fsv2-Serie verfügbar ist, finden Sie unter [Verfügbarkeit der Fsv2-Serie](#fsv2-series).


### <a name="m-seriespreview"></a>M-Serie (Vorschau)

- Die M-Serie ist eine arbeitsspeicheroptimierte Hardwareoption für Workloads, die mehr Arbeitsspeicher und höhere Computegrenzwerte erfordert, als von Gen5 bereitgestellt werden.
- Die M-Serie bietet 29 GB pro V-Kern und 128 V-Kerne. Dadurch erhöht sich der Grenzwert für den Arbeitsspeicher im Vergleich zu Gen5 um das Achtfache auf fast 4 TB.

Um Hardware der M-Serie für ein Abonnement und eine Region zu aktivieren, müssen Sie eine Supportanfrage erstellen. Wenn die Supportanfrage genehmigt wird, erfolgt die Auswahl und die Bereitstellung der M-Serie nach dem gleichen Muster wie bei anderen Hardwaregenerationen. Weitere Informationen zu Regionen, in denen die M-Serie verfügbar ist, finden Sie unter [Verfügbarkeit der M-Serie](#m-series).


### <a name="compute-and-memory-specifications"></a>Spezifikationen zu Compute- und Arbeitsspeicherressourcen


|Hardwaregeneration  |Compute  |Arbeitsspeicher  |
|:---------|:---------|:---------|
|Gen4     |- Intel E5-2673 v3-Prozessoren (Haswell) mit 2,4 GHz<br>- Bereitstellung von bis zu 24 virtuellen Kernen (1 virtueller Kern = 1 physischer Kern)  |- 7 GB pro virtuellem Kern<br>- Bereitstellung von bis zu 168 GB|
|Gen5     |**Bereitgestelltes Computing**<br>- Intel E5-2673 v4-Prozessoren (Broadwell) mit 2,3 GHz<br>- Bereitstellung von bis zu 80 virtuellen Kernen (1 virtueller Kern = 1 Hyperthread)<br><br>**Serverloses Computing**:<br>- Intel E5-2673 v4-Prozessoren (Broadwell) mit 2,3 GHz<br>- Automatische Skalierung auf bis zu 16 virtuelle Kerne (1 virtueller Kern = 1 Hyperthread)|**Bereitgestelltes Computing**<br>- 5,1 GB pro virtuellem Kern<br>- Bereitstellung von bis zu 408 GB<br><br>**Serverloses Computing**:<br>- Automatische Skalierung auf bis zu 24 GB pro V-Kern<br>- Automatische Skalierung auf bis zu 48 GB|
|Fsv2-Serie     |- Intel Xeon Platinum 8168-Prozessoren (Skylake)<br>- Kontinuierliche Turbo-Taktfrequenz von 3,4 GHz für alle Kerne und maximale Turbo-Taktfrequenz für Einzelkerne von 3,7 GHz<br>- Bereitstellung von 72 virtuellen Kernen (1 virtueller Kern = 1 Hyperthread)|- 1,9 GB pro virtuellem Kern<br>- Bereitstellung von 136 GB|
|M-Serie     |- Intel Xeon E7-8890 v3-Prozessoren mit 2,5 GHz<br>- Bereitstellung von 128 virtuellen Kernen (1 virtueller Kern = 1 Hyperthread)|- 29 GB pro virtuellem Kern<br>- Bereitstellung von 3,7 TB|


Weitere Informationen zu Ressourcenlimits finden Sie unter [Ressourcenlimits für Singletons (V-Kern)](sql-database-vcore-resource-limits-single-databases.md) oder [Ressourcenlimits für Pools für elastische Datenbanken (V-Kern)](sql-database-vcore-resource-limits-elastic-pools.md).

### <a name="selecting-a-hardware-generation"></a>Auswählen einer Hardwaregeneration

Sie können im Azure-Portal die Hardwaregeneration für eine SQL-Datenbank oder einen Pool zum Zeitpunkt der Erstellung auswählen, und Sie können die Hardwaregeneration einer vorhandenen SQL-Datenbank oder eines Pools ändern.

**So wählen Sie eine Hardwaregeneration beim Erstellen einer SQL-Datenbank oder eines Pools aus**

Ausführliche Informationen finden Sie unter [Erstellen einer SQL-Datenbank](sql-database-single-database-get-started.md).

Wählen Sie auf der Registerkarte **Grundlagen** im Abschnitt **Compute + Speicher** den Link **Datenbank konfigurieren** und dann den Link **Konfiguration ändern** aus:

  ![Datenbank konfigurieren](media/sql-database-service-tiers-vcore/configure-sql-database.png)

Wählen Sie die gewünschte Hardwaregeneration aus:

  ![Auswählen der Hardware](media/sql-database-service-tiers-vcore/select-hardware.png)


**So ändern Sie die Hardwaregeneration einer vorhandenen SQL-Datenbank oder eines Pools**

Wählen Sie für eine Datenbank auf der Seite „Übersicht“ den Link **Tarif** aus:

  ![Ändern der Hardware](media/sql-database-service-tiers-vcore/change-hardware.png)

Wählen Sie für einen Pool auf der Seite „Übersicht“ die Option **Konfigurieren** aus.

Führen Sie die Schritte zum Ändern der Konfiguration aus, und wählen Sie die Hardwaregeneration wie in den vorherigen Schritten beschrieben aus.

### <a name="hardware-availability"></a>Hardwareverfügbarkeit

#### <a name="gen4gen5"></a>Gen4/Gen5

Neue Gen4-Datenbanken werden in den Regionen „Australien, Osten“ und „Brasilien, Süden“ nicht mehr unterstützt. 

Gen5 ist in den meisten Regionen weltweit verfügbar.

#### <a name="fsv2-series"></a>Fsv2-Serie

Die Fsv2-Serie ist in den folgenden Regionen verfügbar: „Australien, Mitte“, „Australien, Mitte 2“, „Australien, Osten“, „Australien, Südosten“, „Brasilien, Süden“, „Kanada, Mitte“, „Asien, Osten“, „USA, Osten“, „Frankreich, Mitte“, „Indien, Mitte“, „Indien, Westen“, „Südkorea, Mitte“, „Südkorea, Süden“, „Europa, Norden“, „Südafrika, Norden“, „Asien, Südosten“, „Vereinigtes Königreich, Süden“, „Vereinigtes Königreich, Westen“, „Europa, Westen“, „USA, Westen 2“.


#### <a name="m-series"></a>M-Serie

Die M-Serie ist in den folgenden Regionen verfügbar: „USA, Osten“, „Europa, Norden“, „Europa, Westen“, „USA, Westen 2“.
In einigen weiteren Regionen ist die M-Serie möglicherweise eingeschränkt verfügbar. Sie können eine andere Region als die hier aufgelisteten anfordern, die Umsetzung ist jedoch eventuell nicht möglich.

Um die Verfügbarkeit der M-Serie in einem Abonnement zu aktivieren, muss der Zugriff durch das [Erstellen einer neuen Supportanfrage](#create-a-support-request-to-enable-m-series) angefordert werden.


##### <a name="create-a-support-request-to-enable-m-series"></a>Erstellen Sie eine Supportanfrage zum Aktivieren der M-Serie: 

1. Wählen Sie **Hilfe und Support** im Portal aus.
2. Wählen Sie **Neue Supportanfrage** aus.

Geben Sie unter **Grundlagen** die Folgendes an:

1. Wählen Sie unter **Problemtyp** den Eintrag **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.
2. Wählen Sie unter **Abonnement** das Abonnement aus, in dem Sie die M-Serie aktivieren möchten.
3. Wählen Sie unter **Kontingenttyp** den Eintrag **SQL-Datenbank** aus.
4. Wählen Sie **Weiter** aus, um zur Seite **Details** zu wechseln.

Geben Sie auf der Seite **Details** Folgendes an:

5. Wählen Sie im Abschnitt **PROBLEMDETAILS** den Link **Details angeben** aus. 
6. Wählen Sie unter **Kontingenttyp für SQL-Datenbank** den Eintrag **M-Serie** aus.
7. Wählen Sie für **Region** die Region aus, in der die M-Serie aktiviert werden soll.
    Weitere Informationen zu Regionen, in denen die M-Serie verfügbar ist, finden Sie unter [Verfügbarkeit der M-Serie](#m-series).

Genehmigte Supportanfragen werden in der Regel innerhalb von fünf Werktagen bearbeitet.


## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum Erstellen einer SQL-Datenbank finden Sie unter [Erstellen einer SQL-Datenbank mit dem Azure-Portal](sql-database-single-database-get-started.md).
- Die spezifischen Computegrößen und Speichergrößenoptionen für Einzeldatenbanken finden Sie unter [V-Kern-basierte Ressourceneinschränkungen in SQL-Datenbank für Einzeldatenbanken](sql-database-vcore-resource-limits-single-databases.md).
- Die spezifischen Computegrößen und Speichergrößenoptionen für Pools für elastische Datenbanken finden Sie unter [V-Kern-basierte Ressourceneinschränkungen in SQL-Datenbank für Pools für elastische Datenbanken](sql-database-vcore-resource-limits-elastic-pools.md).
- Ausführliche Informationen zu Preisen finden Sie auf der [Preisseite für Azure SQL-Datenbank](https://azure.microsoft.com/pricing/details/sql-database/single/).
