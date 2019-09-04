---
title: Ressourceneinschränkungen für Azure SQL-Datenbank – verwaltete Instanz | Microsoft-Dokumentation
description: Dieser Artikel bietet eine Übersicht über die Ressourceneinschränkungen für verwaltete Azure SQL-Datenbank-Instanzen.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab, jovanpop, sachinp, sstein
ms.date: 08/27/2019
ms.openlocfilehash: 921a14243bc50651358f0df42b88857ab227916d
ms.sourcegitcommit: 388c8f24434cc96c990f3819d2f38f46ee72c4d8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2019
ms.locfileid: "70060634"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Übersicht über Ressourceneinschränkungen für verwaltete Azure SQL-Datenbank-Instanzen

Dieser Artikel bietet eine Übersicht zu den Ressourceneinschränkungen für verwaltete Azure SQL-Datenbank-Instanzen und erläutert, wie Sie eine Anforderung zur Erhöhung dieser Grenzwerte erstellen.

> [!NOTE]
> Unterschiede bei den unterstützten Funktionen und T-SQL-Anweisungen sind unter [Funktionsunterschiede](sql-database-features.md) und [Unterstützung von T-SQL-Anweisungen](sql-database-managed-instance-transact-sql-information.md) zu finden.

## <a name="instance-level-resource-limits"></a>Ressourceneinschränkungen auf Instanzebene

Eine verwaltete Instanz weist Merkmale und Ressourceneinschränkungen auf, die von der zugrunde liegende Infrastruktur und Architektur abhängen. Die Grenzwerte hängen von der Hardwaregeneration und der Dienstebene ab.

### <a name="hardware-generation-characteristics"></a>Merkmale der Hardwaregeneration

Eine verwaltete Azure SQL-Datenbank-Instanz kann auf zwei Hardwaregenerationen bereitgestellt werden: Gen4 und Gen5. Hardwaregenerationen weisen unterschiedliche Merkmale auf, wie in der folgenden Tabelle beschrieben:

|   | **Gen4** | **Gen5** |
| --- | --- | --- |
| Hardware | Intel E5-2673 v3-Prozessoren (Haswell) mit 2,4 GHz, angefügte SSD, virtueller Kern = 1 physischer Kern | Intel E5-2673 v4-Prozessoren (Broadwell) mit 2,3 GHz, schnelle NVMe-SSD, virtueller Kern = 1 LP (Hyperthread) |
| Anzahl der virtuellen Kerne | 8, 16, 24 virtuelle Kerne | 4, 8, 16, 24, 32, 40, 64, 80 virtuelle Kerne |
| Max. Arbeitsspeicher (Verhältnis Arbeitsspeicher/Kerne) | 7 GB pro V-Kern<br/>Fügen Sie weitere virtuelle Kerne hinzu, um mehr Arbeitsspeicher zu erhalten. | 5,1 GB pro virtuellem Kern<br/>Fügen Sie weitere virtuelle Kerne hinzu, um mehr Arbeitsspeicher zu erhalten. |
| Max. In-Memory-OLTP-Arbeitsspeicher | Grenzwert für Instanzen: 3GB pro virtuellem Kern<br/>Grenzwerte für Datenbanken:<br/> – 8 Kerne: 8 GB pro Datenbank<br/> – 16 Kerne: 20 GB pro Datenbank<br/> – 24 Kerne: 36 GB pro Datenbank | Grenzwert für Instanzen: 2,5 GB pro virtuellem Kern<br/>Grenzwerte für Datenbanken:<br/> – 8 Kerne: 13 GB pro Datenbank<br/> – 16 Kerne: 32 GB pro Datenbank |
| Maximal reservierter Instanzspeicher |  Allgemein: 8 TB<br/>Unternehmenskritisch: 1 TB | Allgemein: 8 TB<br/> Unternehmenskritisch: 1 TB, 2 TB oder 4 TB, je nach Anzahl der Kerne |

> [!IMPORTANT]
> - Gen4-Hardware wird eingestellt. Es wird empfohlen, neue verwaltete Instanzen auf Gen5-Hardware bereitzustellen.
> - Gen4-Hardware ist zu diesem Zeitpunkt in den folgenden Regionen verfügbar: „Europa, Norden“, „Europa, Westen“, „USA, Osten“, „USA, Süden-Mitte“, „USA, Norden-Mitte“, „USA, Westen 2“, „USA, Mitte“, „Kanada, Mitte“, „Indien, Süden“, „Asien, Südosten“ und „Südkorea, Mitte“.

### <a name="service-tier-characteristics"></a>Merkmale des Diensttarifs

Die verwaltete Instanz besitzt zwei Dienstebenen: „Universell“ und „Unternehmenskritisch“. Diese Tarife bieten verschiedene Funktionen, die in der folgenden Tabelle beschrieben sind.

| **Feature** | **Allgemeiner Zweck** | **Unternehmenskritisch** |
| --- | --- | --- |
| Anzahl der virtuellen Kerne\* | Gen4: 8, 16, 24<br/>Gen5: 4, 8, 16, 24, 32, 40, 64, 80 | Gen4: 8, 16, 24 <br/> Gen5: 4, 8, 16, 24, 32, 40, 64, 80 |
| Max. Arbeitsspeicherbelegung | Gen4: 56GB – 168GB (7GB/V-Kern)<br/>Gen5: 40,8GB – 408GB (5,1GB/V-Kern)<br/>Fügen Sie weitere virtuelle Kerne hinzu, um mehr Arbeitsspeicher zu erhalten. | Gen4: 56GB – 168GB (7GB/V-Kern)<br/>Gen5: 40,8GB – 408GB (5,1GB/V-Kern)<br/>Fügen Sie weitere virtuelle Kerne hinzu, um mehr Arbeitsspeicher zu erhalten. |
| Max. Größe des reservierten Instanzspeichers | – 2 TB für 4 virtuelle Kerne (nur Gen5)<br/>– 8 TB für andere Größen | Gen4: 1 TB <br/> Gen5: <br/>– 1 TB für 4, 8, 16 virtuelle Kerne<br/>- 2 TB für 24 virtuelle Kerne<br/>- 4 TB für 32, 40, 64, 80 virtuelle Kerne |
| Max. Datenbankgröße | Bestimmt durch die maximale Speichergröße pro Instanz | Bestimmt durch die maximale Speichergröße pro Instanz |
| Max. Anzahl von Datenbanken pro Instanz | 100 | 100 |
| Max. Anzahl von Datenbankdateien pro Instanz | Bis zu 280 | 32.767 Dateien pro Datenbank |
| Max. Dateigröße | 8 TB | 4 TB |
| Daten-/Protokoll-IOPS (ungefähr) | 500 bis 7.500 pro Datei<br/>\*[Erhöhen Sie die Dateigröße, um den IOPS-Wert zu erhöhen](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)| 11K – 110K (1.375/V-Kern)<br/>Fügen Sie weitere virtuelle Kerne hinzu, um die E/A-Leistung zu verbessern. |
| Grenzwert für den Schreibdurchsatz für Protokolle | 3 MB/Sek. pro virtuellem Kern<br/>Max. 22 MB/Sek. pro Instanz | 4 MB/Sek. pro virtuellem Kern<br/>Max. 48 MB/Sek. pro Instanz|
| Datendurchsatz (ungefähr) | 100 bis 250 MB/Sek. pro Datei<br/>\*[Erhöhen Sie die Dateigröße, um die E/A-Leistung zu verbessern.](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes) | – |
| E/A-Speicherlatenz (ungefähr) | 5 – 10 ms | 1 – 2 ms |
| Max. TempDB-Größe | 192 bis 1.920 GB (24 GB pro virtuellem Kern)<br/>Fügen Sie weitere virtuelle Kerne hinzu, um mehr TempDB-Speicherplatz zu erhalten. | Durch die maximale Instanzspeichergröße eingeschränkt. Die Größe der TempDB-Protokolldatei ist derzeit auf 24 GB pro virtuellem Kern beschränkt. |
| In-Memory-OLTP | Nicht unterstützt | Verfügbar |
| Max. Sitzungen | 30000 | 30000 |
| Lesbare Replikate | 0 | 1 |

> [!NOTE]
> - Sowohl die Daten- als auch die Protokolldateigröße in den Benutzer- und Systemdatenbanken sind in der Instanzspeichergröße enthalten, die mit dem Grenzwert für die maximale Speichergröße verglichen wird. Ermitteln Sie mithilfe der Systemansicht <a href="https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-master-files-transact-sql">sys.master_files</a> den von Datenbanken verwendeten Gesamtspeicherplatz. Fehlerprotokolle werden nicht beibehalten und sind nicht in der Größe enthalten. Sicherungen sind nicht in der Speichergröße enthalten.
> - Durchsatz und IOPS hängen auch von der Seitengröße ab, die nicht explizit durch die verwaltete Instanz eingeschränkt wird.
> Sie können ein weiteres lesbares Replikat in einer anderen Azure-Region mithilfe von Autofailover-Gruppen erstellen.

## <a name="supported-regions"></a>Unterstützte Regionen

Eine verwaltete Instanz kann nur in [unterstützten Regionen](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all) erstellt werden. Wenn Sie eine verwaltete Instanz in einer Region erstellen möchten, die derzeit nicht unterstützt wird, können Sie eine [Supportanfrage über das Azure-Portal senden](#obtaining-a-larger-quota-for-sql-managed-instance).

## <a name="supported-subscription-types"></a>Unterstützte Abonnementtypen

Für eine verwaltete Instanz wird derzeit nur die Bereitstellung bei folgenden Abonnementtypen unterstützt:

- [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Nutzungsbasierte Bezahlung](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Clouddienstanbieter (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)
- [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Abonnements mit monatlicher Azure-Gutschrift für Visual Studio-Abonnenten](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)

## <a name="regional-resource-limitations"></a>Regionale Ressourcenbeschränkungen

Unterstützte Abonnementtypen können eine begrenzte Anzahl von Ressourcen pro Region umfassen. Eine verwaltete Instanz weist zwei Standardbeschränkungen pro Azure-Region je nach Abonnementtyp auf:

- **Subnetzlimit**: Die maximale Anzahl von Subnetzen bei Bereitstellung verwalteter Instanzen in einer einzelnen Region.
- **vCore Limit** (Limit für virtuelle Kerne): Die maximale Anzahl von virtuellen Kernen, die in einer einzelnen Region über alle Instanzen hinweg bereitgestellt werden können.

> [!Note]
> Diese Limits sind Standardeinstellungen und keine technischen Einschränkungen. Diese Limits können bei Bedarf erhöht werden, indem Sie eine spezielle [Supportanfrage im Azure-Portal](#obtaining-a-larger-quota-for-sql-managed-instance) erstellen, falls Sie mehr verwaltete Instanzen in der aktuellen Region benötigen. Alternativ können Sie auch neue verwaltete Instanzen in einer anderen Azure-Region erstellen, ohne Supportanfragen zu senden.

In der folgenden Tabelle sind regionale Standardlimits für unterstützte Abonnements angegeben:

|Abonnementtyp| Max. Anzahl von Subnetzen für verwaltete Instanzen | Max number of vCore units* (Maximale Anzahl von virtuellen Kerneinheiten) |
| :---| :--- | :--- |
|Nutzungsbasierte Bezahlung|3|320|
|CSP |8 (15 in manchen Regionen**)|960 (1440 in manchen Regionen**)|
|Pay-as-you-go Dev/Test|3|320|
|Enterprise Dev/Test|3|320|
|EA|8 (15 in manchen Regionen**)|960 (1440 in manchen Regionen**)|
|Visual Studio Enterprise|2 |64|
|Visual Studio Professional und MSDN Platforms|2|32|

\* Berücksichtigen Sie bei der Planung von Bereitstellungen, dass die Dienstebene „Unternehmenskritisch“ (Business Critical, BC) viermal (4-mal) mehr v-Kern-Kapazität erfordert als die Dienstebene „Universell“ (General Purpose, GP). Beispiel:  1 virtueller Kern „Universell“ = 1 V-Kern-Einheit, und 1 virtueller Kern „Unternehmenskritisch“ = 4 V-Kern-Einheiten. Um die Nutzungsanalyse hinsichtlich der Standardgrenzwerte zu vereinfachen, fassen Sie die virtuellen Kerneinheiten für alle Subnetze in der Region zusammen, in der verwaltete Instanzen bereitgestellt werden, und vergleichen Sie die Ergebnisse mit den Grenzwerten für Instanzeinheiten Ihres Abonnementtyps. Der Grenzwert **Max number of vCore units** (Maximale Anzahl von virtuellen Kerneinheiten) gilt für jedes Abonnement in einer Region. Es gibt keinen Grenzwert pro individuellem Subnetz, außer dass die Summe aller in mehreren Subnetzen bereitgestellten virtuellen Kerne niedriger oder gleich der **maximalen Anzahl von virtuellen Kerneinheiten** sein muss.

\*\* Höhere Grenzwerte für Subnetze und virtuelle Kerne sind in den folgenden Regionen verfügbar: „Australien, Osten“, „USA, Osten“, „USA, Osten 2“, „Europa, Norden“, „USA, Süden-Mitte“, „Asien, Südosten“, „Vereinigtes Königreich, Süden“, „Europa, Westen“, „USA, Westen 2“.

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>Abrufen eines größeren Kontingents für verwaltete SQL-Instanz

Wenn Sie mehr verwaltete Instanzen in Ihren aktuellen Regionen benötigen, senden Sie eine Supportanfrage zum Erweitern des Kontingents über das Azure-Portal.
Leiten Sie den Prozess zum Abrufen eines größeren Kontingents auf folgende Weise ein:

1. Öffnen Sie **Hilfe + Support**, und klicken Sie auf **Neue Supportanfrage**.

   ![Hilfe und Support](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. Führen Sie auf der Registerkarte „Grundlagen“ für die neue Supportanfrage die folgenden Schritte aus:
   - Wählen Sie unter **Problemtyp** den Eintrag **Grenzwerte für Dienste und Abonnements (Kontingente)** aus.
   - Wählen Sie unter **Abonnement** Ihr Abonnement aus.
   - Wählen Sie unter **Kontingenttyp** den Eintrag **Verwaltete SQL-Datenbank-Instanz** aus.
   - Wählen Sie unter **Supportplan** Ihren Supportplan aus.

     ![Problemtyp „Kontingent“](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. Klicken Sie auf **Weiter**.
4. Führen Sie auf der Registerkarte **Problem** für die neue Supportanfrage die folgenden Schritte aus:
   - Wählen Sie unter **Schweregrad** den Schweregrad des Problems aus.
   - Geben Sie unter **Details** zusätzliche Informationen zu Ihrem Problem an, einschließlich Fehlermeldungen.
   - Fügen Sie unter **Dateiupload** eine Datei mit weiteren Informationen an (bis zu 4 MB).

     ![Problemdetails](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > Eine gültige Anforderung sollte Folgendes enthalten:
     > - Region, in der der Grenzwert für Abonnements erhöht werden muss.
     > - Erforderliche Anzahl von virtuellen Kernen pro Dienstebene in vorhandenen Subnetzen nach Erhöhung des Kontingents (wenn eines der vorhandenen Subnetze erweitert werden muss).
     > - Erforderliche Anzahl neuer Subnetze und Gesamtanzahl der virtuellen Kerne pro Dienstebene in den neuen Subnetzen (wenn Sie verwaltete Instanzen in neuen Subnetzen bereitstellen müssen).

5. Klicken Sie auf **Weiter**.
6. Geben Sie auf der Registerkarte „Kontaktinformationen“ für die neue Supportanfrage Ihre bevorzugte Kontaktmethode (E-Mail oder Telefon) und die Kontaktdetails ein.
7. Klicken Sie auf **Create**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zur verwalteten Instanz finden Sie unter [Was ist eine verwaltete Instanz?](sql-database-managed-instance.md).
- Preisinformationen finden Sie unter [Preise – verwaltete Azure SQL-Datenbank-Instanzen ](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- Im [Schnellstarthandbuch](sql-database-managed-instance-get-started.md) erfahren Sie, wie Sie Ihre erste verwaltete Instanz erstellen.
