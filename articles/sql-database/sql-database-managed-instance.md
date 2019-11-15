---
title: Übersicht über verwaltete SQL-Instanzen
description: In diesem Artikel wird die verwaltete Azure SQL-Datenbank-Instanz beschrieben.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: sstein, carlrab, vanto
ms.date: 11/04/2019
ms.openlocfilehash: 6c5b913835b2080f30ff3dd73e6a59c1043ecf5d
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73823291"
---
# <a name="what-is-azure-sql-database-managed-instance"></a>Was ist eine verwaltete Azure SQL-Datenbank-Instanz?

Eine verwaltete Instanz ist eine neue Bereitstellungsoption von Azure SQL-Datenbank, die für nahezu uneingeschränkte Kompatibilität mit der aktuellen lokalen SQL Server-Datenbank-Engine (Enterprise Edition) sorgt. Darüber hinaus bietet sie eine native Implementierung eines [virtuellen Netzwerks (VNET)](../virtual-network/virtual-networks-overview.md) zur Behebung allgemeiner Sicherheitsrisiken sowie ein vorteilhaftes [Geschäftsmodell](https://azure.microsoft.com/pricing/details/sql-database/) für Kunden mit lokaler SQL Server-Instanz. Mit dem Bereitstellungsmodell für die verwaltete Instanz können bestehende SQL Server-Kunden ihre lokalen Anwendungen mit minimalen Änderungen an den Anwendungen und Datenbanken per Lift & Shift zur Cloud migrieren. Gleichzeitig behält die Bereitstellungsoption für die verwaltete Instanz (automatisches Patchen und automatische Versionsupdates, [automatische Sicherungen](sql-database-automated-backups.md), [Hochverfügbarkeit](sql-database-high-availability.md)) bei, die den Verwaltungsaufwand und die Gesamtkosten drastisch reduzieren.

> [!IMPORTANT]
> Eine Liste der Regionen, in denen die Bereitstellungsoption für eine verwaltete Instanzen derzeit verfügbar sind, finden Sie unter [Unterstützte Regionen](sql-database-managed-instance-resource-limits.md#supported-regions).

In der folgenden Abbildung sind die wichtigsten Features der verwalteten Instanz dargestellt:

![Wichtigste Features](./media/sql-database-managed-instance/key-features.png)

Das Bereitstellungsmodell für die verwaltete Instanz ist für Kunden konzipiert, die eine große Anzahl von Apps aus einer selbst erstellten oder über ISVs bereitgestellten lokalen Umgebung oder IaaS-Umgebung mit möglichst geringem Migrationsaufwand in eine vollständig verwaltete PaaS-Cloudumgebung migrieren möchten. Unter Verwendung des vollständig automatisierten [Data Migration Service (DMS)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) in Azure können Kunden ihren lokalen SQL Server per Lift & Shift zu einer verwalteten Instanz migrieren, die Kompatibilität mit dem lokalen SQL Server und vollständige Isolation von Kundeninstanzen mit nativer VNET-Unterstützung bietet.  Mit Software Assurance können Sie die vorhandenen Lizenzen der Kunden mit dem [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) zu ermäßigten Preisen für eine verwaltete Instanz austauschen.  Eine verwaltete Instanz ist das beste Migrationsziel in der Cloud für SQL Server-Instanzen, die hohe Sicherheit und eine umfangreiche Programmieroberfläche erfordern.

Das Bereitstellungsmodell für die verwaltete Instanz strebt durch einen gestaffelten Releaseplan eine nahezu 100%ige Oberflächenkompatibilität mit dem neuesten lokalen SQL Server-Release an.

Anhaltspunkte für die Entscheidung zwischen den verschiedenen Bereitstellungsoptionen für die Azure SQL-Datenbank zwischen Einzeldatenbank, Pooldatenbank, einer verwalteten Instanz und SQL Server auf einem virtuellen Computer finden Sie unter [Auswählen der richtigen Version von SQL Server in Azure](sql-database-paas-vs-sql-server-iaas.md).

## <a name="key-features-and-capabilities"></a>Wichtige Features und Funktionen

Eine verwaltete Instanz kombiniert die besten Features, die sowohl in Azure SQL-Datenbank als auch SQL Server-Datenbank-Engine verfügbar sind.

> [!IMPORTANT]
> Eine verwaltete Instanz wird mit allen Features der neuesten Version von SQL Server ausgeführt – einschließlich Onlinevorgängen, automatischer Plankorrekturen und anderer Leistungsverbesserungen für Unternehmen. Einen Vergleich der verfügbaren Features finden Sie unter [Funktionsvergleich: Azure SQL-Datenbank und SQL Server](sql-database-features.md).

| **PaaS-Vorteile** | **Geschäftskontinuität** |
| --- | --- |
|Kein Kauf und keine Verwaltung von Hardware <br>Kein zusätzlicher Aufwand für die Verwaltung der zugrunde liegenden Infrastruktur <br>Schnelle Bereitstellung und Dienstskalierung <br>Automatische Patches und Versionsupgrades <br>Integration in andere PaaS-Datendienste |Betriebszeit-SLA von 99,99 %  <br>Integrierte [Hochverfügbarkeit](sql-database-high-availability.md) <br>Schutz der Daten durch [automatische Sicherungen](sql-database-automated-backups.md) <br>Vom Kunden konfigurierbare Aufbewahrungsdauer für Sicherungen <br>Vom Benutzer initiierte [Sicherungen](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql?view=azuresqldb-mi-current) <br>Funktion für [Point-in-Time-Datenbankwiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore) |
|**Sicherheit und Konformität** | **Verwaltung**|
|Isolierte Umgebung ([VNET-Integration](sql-database-managed-instance-connectivity-architecture.md), Dienst mit einzelnem Mandanten, dedizierte Compute- und Speicherressourcen) <br>[Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)<br>[Azure AD-Authentifizierung](sql-database-aad-authentication.md), Unterstützung für einmaliges Anmelden <br> <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">Azure AD-Serverprinzipale (Anmeldungen)</a>  <br>Gleiche Kompatibilitätsstandards wie für Azure SQL-Datenbank <br>[SQL-Überwachung](sql-database-managed-instance-auditing.md) <br>[Advanced Threat Protection für Azure SQL-Datenbank](sql-database-managed-instance-threat-detection.md) |Azure Resource Manager-API zur Automatisierung der Dienstbereitstellung und -skalierung <br>Funktionen des Azure-Portals für die manuelle Dienstbereitstellung und -skalierung <br>Data Migration Service

> [!IMPORTANT]
> Azure SQL-Datenbank (alle Bereitstellungsoptionen) wurde anhand einer Reihe von Konformitätsstandards zertifiziert. Weitere Informationen finden Sie im [Microsoft Azure Trust Center](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942), wo die aktuellste Liste von Compliance-Zertifizierungen für SQL-Datenbank angezeigt wird.

Die wichtigsten Features der verwalteten Instanz sind in der folgenden Tabelle angegeben:

|Feature | BESCHREIBUNG|
|---|---|
| SQL Server-Version/-Build | SQL Server-Datenbank-Engine (letzte stabile Version) |
| Verwaltete automatisierte Sicherungen | Ja |
| Integrierte Überwachung und Metriken von Instanzen und Datenbanken | Ja |
| Automatische Softwarepatches | Ja |
| Neueste Features der Datenbank-Engine | Ja |
| Anzahl der Datendateien (ROWS) pro Datenbank | Mehrere |
| Anzahl der Protokolldateien (LOG) pro Datenbank | 1 |
| VNET – Azure Resource Manager-Bereitstellung | Ja |
| VNET – Klassisches Bereitstellungsmodell | Nein |
| Portal-Unterstützung | Ja|
| Integrierter Integrationsdienst (SSIS) | Nein – SSIS ist Teil von [Azure Data Factory PaaS](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) |
| Integrierter Analysedienst (SSAS) | Nein – SSAS ist ein separater [PaaS](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) |
| Integrierter Berichterstellungsdienst (SSRS) | Nein – Power BI oder SSRS IaaS verwenden |
|||

## <a name="vcore-based-purchasing-model"></a>vCore-basiertes Kaufmodell

Das [vCore-basierte Kaufmodell](sql-database-service-tiers-vcore.md) für verwaltete Instanzen bietet Ihnen mehr Flexibilität, Kontrolle und Transparenz sowie eine unkomplizierte Möglichkeit, Ihre lokalen Workloadanforderungen in der Cloud zu realisieren. Mit diesem Modell können Sie Computeressourcen, Arbeitsspeicher und Speicher entsprechend Ihren Workloadanforderungen ändern. Das V-Kern-Modell ermöglicht mit dem [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) zudem Einsparungen von bis zu 30 Prozent.

Beim V-Kern-Modell können Sie verschiedene Hardwaregenerationen auswählen.

- **Gen4**: Logische CPUs basierend auf Intel-Prozessoren vom Typ E5-2673 v3 (Haswell) mit 2,4 GHz, angefügter SSD, physischen Kernen, 7 GB RAM pro Kern und Computegrößen zwischen 8 und 24 virtuellen Kernen.
- **Gen5**: Logische CPUs basierend auf Intel-Prozessoren vom Typ E5-2673 v4 (Broadwell) mit 2,3 GHz,schnelle NVMe SSD, logischer Kern mit Hyperthreading und Computegrößen zwischen 4 und 80 Kernen.

Weitere Informationen zu den Unterschieden zwischen Hardwaregenerationen finden Sie unter [Ressourceneinschränkungen für verwaltete Instanzen](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).

> [!IMPORTANT]
> Neue Gen4-Datenbanken werden in den Regionen „Australien, Osten“ und „Brasilien, Süden“ nicht mehr unterstützt.

## <a name="managed-instance-service-tiers"></a>Dienstebenen für eine verwaltete Instanz

Eine verwaltete Instanz ist in zwei Dienstebenen verfügbar:

- **Universell**: Entwickelt für Anwendungen mit typischen Leistungs- und E/A-Latenzanforderungen.
- **Unternehmenskritisch**: Entwickelt für Anwendungen mit niedrigen E/A-Latenzanforderungen und minimalen Auswirkungen auf zugrunde liegende Wartungsvorgänge für die Workload.

Beide Dienstebenen garantieren eine Verfügbarkeit von 99,99 Prozent und ermöglichen es Ihnen, die Speichergröße und Computekapazität unabhängig voneinander auszuwählen. Weitere Informationen zur Hochverfügbarkeitsarchitektur von Azure SQL-Datenbank finden Sie unter [Hochverfügbarkeit und Azure SQL-Datenbank](sql-database-high-availability.md).

### <a name="general-purpose-service-tier"></a>Universelle Dienstebene

Folgende wichtige Merkmale kennzeichnen die universelle Dienstebene:

- Entwurf für den Großteil der Geschäftsanwendungen mit typischen Leistungsanforderungen
- Azure Blob Storage mit hoher Leistung (8 TB)
- Integrierte [Hochverfügbarkeit](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) basierend auf zuverlässigem Azure Blob Storage und [Azure Service Fabric](../service-fabric/service-fabric-overview.md)

Weitere Informationen finden Sie unter [Speicherebene im Tarif „Universell“](https://medium.com/azure-sqldb-managed-instance/file-layout-in-general-purpose-azure-sql-managed-instance-cf21fff9c76c) und [Bewährte Methoden und Aspekte der Speicherleistung für verwaltete Instanz (Universell)](https://blogs.msdn.microsoft.com/sqlcat/2018/07/20/storage-performance-best-practices-and-considerations-for-azure-sql-db-managed-instance-general-purpose/).

Weitere Informationen zu den Unterschieden zwischen Dienstebenen finden Sie unter [Ressourceneinschränkungen für verwaltete Instanzen](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

### <a name="business-critical-service-tier"></a>Dienstebene „Unternehmenskritisch“

Die unternehmenskritische Dienstebene wurde für Anwendungen mit hohen E/A-Anforderungen konzipiert. Sie bietet höchste Resilienz gegenüber Ausfällen durch mehrere isolierte Replikate.

Folgende wichtige Merkmale kennzeichnen die Dienstebene „Unternehmenskritisch“:

- Konzipiert für Geschäftsanwendungen mit höchster Leistung und Hochverfügbarkeitsanforderungen
- Enthält einen äußerst schnellen lokalen SSD-Speicher (bis zu 1 TB bei Gen 4 und bis zu 4 TB bei Gen 5)
- Integrierte [Hochverfügbarkeit](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) basierend auf [Always On-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) und [Azure Service Fabric](../service-fabric/service-fabric-overview.md)
- Integriertes zusätzliches [Datenbankreplikat mit Schreibschutz](sql-database-read-scale-out.md), das für Berichte und andere schreibgeschützte Workloads verwendet werden kann
- [In-Memory-OLTP](sql-database-in-memory.md), das für Workloads mit hohen Leistungsanforderungen verwendet werden kann  

Weitere Informationen zu den Unterschieden zwischen Dienstebenen finden Sie unter [Ressourceneinschränkungen für verwaltete Instanzen](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).


## <a name="managed-instance-management-operations"></a>Verwaltungsvorgänge für verwaltete Instanzen

Azure SQL-Datenbank verfügt über Verwaltungsvorgänge, die Sie zum automatischen Bereitstellen von neuen verwalteten Instanzen, Aktualisieren von Instanzeigenschaften und Löschen von Instanzen (falls diese nicht mehr benötigt werden) verwenden können. Dieser Abschnitt enthält Informationen zu Verwaltungsvorgängen und deren typischer Dauer.

Zur Unterstützung von [Bereitstellungen in virtuellen Azure-Netzwerken (VNETs)](../virtual-network/virtual-network-for-azure-services.md#deploy-azure-services-into-virtual-networks) und zur Sicherstellung der Isolation und Sicherheit für Kunden werden für verwaltete Instanzen [virtuelle Cluster](sql-database-managed-instance-connectivity-architecture.md#high-level-connectivity-architecture) verwendet. Hierbei handelt es sich um einen dedizierten Satz isolierter virtueller Computer, die im VNET-Subnetz des Kunden bereitgestellt werden. Praktisch jede Bereitstellung einer verwalteten Instanz in einem leeren Subnetz führt dazu, dass ein neuer virtueller Cluster erstellt wird.

Nachfolgende Vorgänge für bereitgestellte verwaltete Instanzen können auch Auswirkungen auf den zugrunde liegenden virtuellen Cluster haben. Dies wirkt sich auf die Dauer von Verwaltungsvorgängen aus, da die Bereitstellung von zusätzlichen virtuellen Computern mit Mehraufwand verbunden ist. Dieser muss berücksichtigt werden, wenn Sie neue Bereitstellungen oder Updates vorhandener verwalteter Instanzen planen.

Alle Verwaltungsvorgänge können wie folgt kategorisiert werden:

- Instanzbereitstellung (Erstellung neuer Instanzen) 
- Instanzupdate (Änderung von Instanzeigenschaften, z. B. V-Kerne oder reservierter Speicher)
- Instanzlöschung

Normalerweise dauern Vorgänge in virtuellen Clustern am längsten. Die Dauer der Vorgänge in virtuellen Clustern variiert. Unten sind die Werte angegeben, die für vorhandene Diensttelemetriedaten normalerweise zu erwarten sind:

- Erstellung eines virtuellen Clusters. Dies ist ein synchroner Schritt der Vorgänge für die Instanzverwaltung. **90 % der Vorgänge werden innerhalb von vier Stunden abgeschlossen**.
- Änderung der Größe des virtuellen Clusters (Vergrößerung oder Verkleinerung). Die Vergrößerung ist ein synchroner Schritt, und die Verkleinerung wird asynchron durchgeführt (ohne Auswirkung auf die Dauer von Instanzverwaltungsvorgängen). **90 % der Clustervergrößerungen werden in weniger als 2,5 Stunden abgeschlossen**.
- Löschung eines virtuellen Clusters. Der Löschvorgang ist ein asynchroner Schritt, der in einem leeren virtuellen Cluster aber auch [manuell initiiert](sql-database-managed-instance-delete-virtual-cluster.md) werden kann. In diesem Fall wird der Vorgang synchron durchgeführt. **90 % der Löschungen virtueller Cluster werden innerhalb von 1,5 Stunden abgeschlossen**.

Darüber hinaus kann die Verwaltung von Instanzen auch einen der Vorgänge in gehosteten Datenbanken umfassen. Dies führt zu längeren Dauern:

- Anfügen von Datenbankdateien aus Azure Storage. Dies ist ein synchroner Schritt, z. B. Compute (V-Kern) oder das zentrale Hoch- oder Herunterskalieren im Tarif „Universell“. **90 % dieser Vorgänge werden innerhalb von fünf Minuten abgeschlossen**.
- Seeding der Always On-Verfügbarkeitsgruppe. Dies ist ein synchroner Schritt, z. B. Compute (V-Kern) oder die Skalierung des Speichers im Tarif „Unternehmenskritisch“ sowie die Änderung der Dienstebene von „Universell“ in „Unternehmenskritisch“ (oder umgekehrt). Die Dauer dieses Vorgangs ist proportional zur Gesamtgröße der Datenbank sowie zur aktuellen Datenbankaktivität (Anzahl aktiver Transaktionen). Aufgrund der Datenbankaktivität beim Aktualisieren einer Instanz kann die Gesamtdauer erheblich variieren. **90 % dieser Vorgänge werden mindestens mit 220 GB pro Stunde durchgeführt**.

In der folgenden Tabelle sind die Vorgänge und die typischen Gesamtdauern zusammengefasst:

|Category  |Vorgang  |Segment mit langer Ausführungsdauer  |Geschätzte Dauer  |
|---------|---------|---------|---------|
|**Bereitstellung** |Erste Instanz in einem leeren Subnetz|Erstellung eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von vier Stunden abgeschlossen|
|Bereitstellung |Erste Instanz einer anderen Hardwaregeneration in einem nicht leeren Subnetz (z. B. erste Gen5-Instanz in einem Subnetz mit Gen4-Instanzen)|Erstellung eines virtuellen Clusters*|90 % der Vorgänge werden innerhalb von vier Stunden abgeschlossen|
|Bereitstellung |Erstellung der ersten Instanz mit vier V-Kernen in einem leeren oder nicht leeren Subnetz|Erstellung eines virtuellen Clusters**|90 % der Vorgänge werden innerhalb von vier Stunden abgeschlossen|
|Bereitstellung |Nachfolgende Instanzerstellung im nicht leeren Subnetz (2. Instanz, 3. Instanz usw.)|Änderung der Größe eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von 2,5 Stunden abgeschlossen|
|**Aktualisieren** |Änderung der Instanzeigenschaft (Administratorkennwort, AAD-Anmeldung, Azure-Hybridvorteil-Flag)|–|Bis zu 1 Minute|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren des Instanzspeichers (Dienstebene „Universell“)|- Änderung der Größe eines virtuellen Clusters<br>- Anfügung von Datenbankdateien|90 % der Vorgänge werden innerhalb von 2,5 Stunden abgeschlossen|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren des Instanzspeichers (Tarif „Unternehmenskritisch“)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Universell)|- Änderung der Größe eines virtuellen Clusters<br>- Anfügung von Datenbankdateien|90 % der Vorgänge werden innerhalb von 2,5 Stunden abgeschlossen|
|Aktualisieren |Zentrales Hoch-/Herunterskalieren der Computekapazität (V-Kerne) (Unternehmenskritisch)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen|
|Aktualisieren |Zentrales Herunterskalieren einer Instanz auf vier V-Kerne (Universell)|- Änderung der Größe eines virtuellen Clusters (beim ersten Mal ist ggf. die Erstellung eines virtuellen Clusters erforderlich**)<br>- Anfügung von Datenbankdateien|90 % der Vorgänge werden innerhalb von 4 Stunden und 5 Minuten abgeschlossen**|
|Aktualisieren |Zentrales Herunterskalieren einer Instanz auf vier V-Kerne (Universell)|- Änderung der Größe eines virtuellen Clusters (beim ersten Mal ist ggf. die Erstellung eines virtuellen Clusters erforderlich**)<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 4 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen|
|Aktualisieren |Änderung der Instanzdienstebene („Universell“ in „Unternehmenskritisch“ und umgekehrt)|- Änderung der Größe eines virtuellen Clusters<br>- Seeding der Always On-Verfügbarkeitsgruppe|90 % der Vorgänge werden innerhalb von 2,5 Stunden zzgl. der Zeit für das Seeding aller Datenbanken (220 GB/Stunde) abgeschlossen|
|**Löschung**|Instanzlöschung|Log Tail-Sicherung für alle Datenbanken|90 % der Vorgänge werden innerhalb einer Minute abgeschlossen.<br>Hinweis: Wenn die letzte Instanz im Subnetz gelöscht wird, wird bei diesem Vorgang das Löschen des virtuellen Clusters nach 12 Stunden eingeplant.***|
|Löschen|Löschung eines virtuellen Clusters (als vom Benutzer initiierter Vorgang)|Löschung eines virtuellen Clusters|90 % der Vorgänge werden innerhalb von 1,5 Stunden abgeschlossen|

\* Der virtuelle Cluster wird pro Hardwaregeneration erstellt.

\*\* Die Option für die Bereitstellung von vier V-Kernen wurde im Juni 2019 eingeführt und erfordert eine neue Version des virtuellen Clusters. Falls Sie im Zielsubnetz über Instanzen verfügen, die alle vor dem 12. Juni erstellt wurden, wird automatisch ein neuer virtueller Cluster bereitgestellt, um vier V-Kern-Instanzen zu hosten.

\*\*\* Bei der aktuellen Konfiguration wird „12 Stunden“ verwendet, aber da sich dies in Zukunft ändern kann, sollten Sie hierfür keine festen Abhängigkeiten einrichten. Falls Sie einen virtuellen Cluster früher löschen müssen (z. B. um das Subnetz freizugeben), helfen Ihnen die Informationen unter [Löschen eines Subnetzes nach Löschen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-delete-virtual-cluster.md) weiter.

### <a name="instance-availability-during-management"></a>Instanzverfügbarkeit während der Verwaltung

Verwaltete Instanzen sind bei Bereitstellungs-und Löschvorgängen für Clientanwendungen nicht verfügbar.

Verwaltete Instanzen sind bei Updatevorgängen verfügbar, aber es kommt zu einem kurzen Ausfall aufgrund des Failovers, das am Ende von Updates ausgeführt wird. Dies dauert normalerweise maximal zehn Sekunden.

> [!IMPORTANT]
> Die Dauer eines Failovers kann bei Transaktionen mit langer Ausführungsdauer, die für die Datenbanken durchgeführt werden, aufgrund einer [verlängerten Wiederherstellungszeit](sql-database-accelerated-database-recovery.md#the-current-database-recovery-process) erheblich variieren. Daher raten wir Ihnen davon ab, die Compute- oder Speicherkapazität der verwalteten Azure SQL-Datenbank-Instanzen zu skalieren oder die Dienstebene gleichzeitig mit den Transaktionen mit langer Ausführungsdauer (Datenimport, Datenverarbeitungsaufträge, Neuerstellung des Index usw.) zu ändern. Beim Datenbankfailover, das am Ende des Vorgangs ausgeführt wird, werden laufende Transaktionen abgebrochen. Dies führt zu einer verlängerten Wiederherstellungszeit.

Die [Schnellere Datenbankwiederherstellung](sql-database-accelerated-database-recovery.md) ist für verwaltete Azure SQL-Datenbank-Instanzen derzeit nicht verfügbar. Nach der Aktivierung wird mit diesem Feature die Variabilität der Failoverzeit erheblich reduziert. Dies gilt auch für Transaktionen mit langer Ausführungsdauer.



## <a name="advanced-security-and-compliance"></a>Erweiterte Sicherheit und Konformität

Eine Bereitstellungsoption für die verwaltete Instanz kombiniert erweiterte Sicherheitsfeatures, die von der Azure-Cloud und SQL Server-Datenbank-Engine bereitgestellt werden.

### <a name="managed-instance-security-isolation"></a>Sicherheitsisolation bei verwalteten Instanzen

Eine verwaltete Instanz bietet zusätzliche Sicherheitsisolation von anderen Mandanten in der Azure-Cloud. Die Sicherheitsisolation umfasst Folgendes:

- [Native Implementierung von und Konnektivität mit virtuellen Netzwerken](sql-database-managed-instance-connectivity-architecture.md) in der lokalen Umgebung unter Verwendung von Azure ExpressRoute oder VPN Gateway.
- Der SQL-Endpunkt wird in einer Standardbereitstellung nur über eine private IP-Adresse verfügbar gemacht, sodass sichere Verbindungen von privaten Azure-Netzwerken oder hybriden Netzwerken möglich sind.
- Einzelner Mandant mit dedizierter zugrunde liegender Infrastruktur (Compute, Speicher).

Das folgende Diagramm zeigt verschiedene Konnektivitätsoptionen für Ihre Anwendungen:

![Hochverfügbarkeit](./media/sql-database-managed-instance/application-deployment-topologies.png)  

Weitere Informationen zur VNET-Integration und Durchsetzung von Netzwerkrichtlinien auf Subnetzebene finden Sie unter [VNET-Architektur für eine verwaltete Instanz](sql-database-managed-instance-connectivity-architecture.md) und [Herstellen einer Verbindung zwischen einer Anwendung und einer verwalteten Instanz](sql-database-managed-instance-connect-app.md).

> [!IMPORTANT]
> Platzieren Sie mehrere verwaltete Instanzen im gleichen Subnetz, wo immer dies durch Ihre Sicherheitsanforderungen erlaubt ist, da Sie dadurch zusätzliche Vorteile erzielen können. Das Zusammenstellen von Instanzen im gleichen Subnetz wird die Wartung der Netzwerkinfrastruktur erheblich vereinfachen und die Zeit für die Instanzbereitstellung verkürzen, da eine lange Bereitstellungsdauer mit den Kosten für die Bereitstellung der ersten verwalteten Instanz in einem Subnetz verbunden ist.

### <a name="azure-sql-database-security-features"></a>Sicherheitsfeatures für Azure SQL-Datenbank

Azure SQL-Datenbank bietet eine Reihe von erweiterten Sicherheitsfeatures, die zum Schutz Ihrer Daten verwendet werden können.

- Die [Überwachung verwalteter Instanzen](sql-database-managed-instance-auditing.md) verfolgt Datenbankereignisse nach und schreibt sie in eine Überwachungsprotokolldatei in Ihrem Azure-Speicherkonto. Die Überwachung kann dabei helfen, gesetzliche Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftliche Probleme oder mutmaßliche Sicherheitsverstöße hinweisen können.
- Datenverschlüsselung in Aktion: Eine verwaltete Instanz schützt Ihre Daten durch die Verschlüsselung für Daten während der Übertragung mit Transport Layer Security. Neben Transport Layer Security bietet eine Bereitstellungsoption für die verwaltete Instanz mit [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine) einen Schutz für sensible Daten bei der Übertragung, im Ruhezustand und während der Abfrageverarbeitung. Always Encrypted ist das branchenweit erste System, das einen beispiellosen Schutz von Daten gegen Sicherheitsverletzungen wie dem Diebstahl wichtiger Daten bietet. Beispielsweise werden mit Always Encrypted Kreditkartennummern immer verschlüsselt in der Datenbank gespeichert, selbst während der Abfrageverarbeitung. Gleichzeitig wird autorisierten Mitarbeitern oder Anwendungen, die diese Daten verarbeiten müssen, die Entschlüsselung zum Zeitpunkt der Verwendung erlaubt.
- Mit [Advanced Threat Protection](sql-database-managed-instance-threat-detection.md) wird die [Überwachung](sql-database-managed-instance-auditing.md) vervollständigt, indem der Dienst um eine zusätzliche Security Intelligence-Ebene erweitert wird, die ungewöhnliche und eventuell schädliche Versuche, auf Datenbanken zuzugreifen oder sie zu missbrauchen, erkennt. Sie werden vor verdächtigen Aktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen mit Einschleusung von SQL-Befehlen und anomalen Datenbankzugriffsmustern gewarnt. Advanced Threat Protection-Warnungen können in [Azure Security Center](https://azure.microsoft.com/services/security-center/) angezeigt werden und bieten Detailinformationen zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann.  
- Die [dynamische Datenmaskierung](/sql/relational-databases/security/dynamic-data-masking) schränkt die Offenlegung sensibler Daten ein, indem sie für nicht berechtigte Benutzer maskiert werden. Mit der dynamischen Datenmaskierung können Sie mit minimalen Auswirkungen auf die Anwendungsschicht festlegen, wie viel von den sensiblen Daten offengelegt werden soll. Dies trägt zur Verhinderung des unbefugten Zugriffs auf sensible Daten bei. Es handelt sich um eine richtlinienbasierte Sicherheitsfunktion, die die sensiblen Daten im Resultset einer Abfrage in festgelegten Datenbankfeldern ausblendet, ohne dass die Daten in der Datenbank geändert werden.
- Bei der [Sicherheit auf Zeilenebene](/sql/relational-databases/security/row-level-security) können Sie den Zugriff auf Zeilen in einer Datenbanktabelle basierend auf den Merkmalen des Benutzers steuern, der eine Abfrage ausführt (z.B. Gruppenmitgliedschaft oder Ausführungskontext). Die Sicherheit auf Zeilenebene (Row-Level Security, RLS) vereinfacht das Entwerfen und Programmieren der Sicherheit in Ihrer Anwendung. Mit RLS können Sie den Zugriff auf Datenzeilen einschränken. So können Sie beispielsweise sicherstellen, dass Mitarbeiter nur auf die Datenzeilen zugreifen können, die für ihre Abteilung relevant sind, oder den Datenzugriff ausschließlich auf die relevanten Daten beschränken.
- [Transparent Data Encryption (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) verschlüsselt Datendateien einer verwalteten Instanz. Dies wird als Verschlüsselung ruhender Daten bezeichnet. TDE führt die E/A-Verschlüsselung und -Entschlüsselung der Daten- und Protokolldateien in Echtzeit durch. Die Verschlüsselung verwendet einen Datenbank-Verschlüsselungsschlüssel (DEK), der im Startdatensatz der Datenbank gespeichert wird und während der Wiederherstellung zur Verfügung steht. Sie können alle Ihre Datenbanken in der verwalteten Instanz mit transparenter Datenverschlüsselung schützen. TDE ist die bewährte SQL Server-Verschlüsselungstechnologie für ruhende Daten, die für viele Konformitätsstandards zum Schutz vor Diebstahl von Speichermedien erforderlich ist.

Die Migration einer verschlüsselten Datenbank zur verwalteten Instanz wird über den Azure Database Migration Service (DMS) oder die native Wiederherstellung unterstützt. Wenn Sie planen, eine verschlüsselte Datenbank mittels nativer Wiederherstellung zu migrieren, ist die Migration des vorhandenen TDE-Zertifikats vom lokalen SQL-Server oder von SQL Server in einen virtuellen Computer zur verwalteten Instanz erforderlich. Weitere Informationen zu den Migrationsoptionen finden Sie unter [Migration einer SQL Server-Instanz zu einer verwalteten Instanz](sql-database-managed-instance-migrate.md).

## <a name="azure-active-directory-integration"></a>Azure Active Directory-Integration

Die Bereitstellungsoption für die verwaltete Instanz unterstützt herkömmliche SQL Server-Datenbank-Engine-Anmeldungen und in Azure Active Directory (AAD) integrierte Anmeldungen. Azure AD-Serverprinzipale (Anmeldungen) (**öffentliche Vorschau**) sind die Azure-Cloudversion von lokalen Datenbankanmeldungen, die Sie in Ihrer lokalen Umgebung verwenden. Azure AD-Serverprinzipale (Anmeldungen) ermöglichen Ihnen das Angeben von Benutzern und Gruppen von Ihrem Azure Active Directory-Mandanten als Prinzipale, die auch tatsächlich im Bereich der Instanz liegen. So können Sie jeden Vorgang auf Instanzebene ausführen – auch datenbankübergreifende Abfragen innerhalb derselben verwalteten Instanz.

Eine neue Syntax wird eingeführt, um Azure AD-Serverprinzipale (Anmeldungen) zu erstellen: **FROM EXTERNAL PROVIDER**. Weitere Informationen zur Syntax finden Sie unter <a href="/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current">CREATE LOGIN</a>, und lesen Sie auch den Artikel [Bereitstellen eines Azure Active Directory-Administrators für Ihre verwaltete SQL-Datenbank-Instanz](sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-managed-instance).

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory-Integration und Multi-Factor Authentication

Die Bereitstellungsoption für die verwaltete Instanz ermöglicht über die [Azure Active Directory-Integration](sql-database-aad-authentication.md) eine zentrale Verwaltung von Identitäten der Datenbankbenutzer und anderer Microsoft-Dienste. Diese Funktion vereinfacht die Berechtigungsverwaltung und erhöht die Sicherheit. Azure Active Directory unterstützt [Multi-Factor Authentication](sql-database-ssms-mfa-authentication-configure.md) (MFA), um die Daten- und Anwendungssicherheit zu erhöhen, während gleichzeitig einmaliges Anmelden unterstützt wird.

### <a name="authentication"></a>Authentication

Die Authentifizierung einer verwalteten Instanz bezieht sich darauf, wie Benutzer ihre Identität beim Herstellen der Verbindung mit der Datenbank nachweisen. SQL-Datenbank unterstützt zwei Arten der Authentifizierung:  

- **SQL-Authentifizierung**:

  Diese Authentifizierungsmethode verwendet einen Benutzernamen und ein Kennwort.
- **Azure Active Directory-Authentifizierung**:

  Diese Authentifizierungsmethode verwendet von Azure Active Directory verwaltete Identitäten und wird für verwaltete und integrierte Domänen unterstützt. Verwenden Sie immer die Active Directory-Authentifizierung (integrierte Sicherheit), [sofern dies möglich ist](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

### <a name="authorization"></a>Authorization

Autorisierung bezieht sich darauf, welche Aufgaben ein Benutzer innerhalb einer Azure SQL-Datenbank ausführen kann. Dies wird durch datenbankbezogene Rollenmitgliedschaften und Objektebenenberechtigungen Ihres Benutzerkontos gesteuert. Eine verwaltete Instanz enthält die gleichen Autorisierungsfunktionen wie SQL Server 2017.

## <a name="database-migration"></a>Datenbankmigration

Die Bereitstellungsoption für die verwaltete Instanz zielt auf Benutzerszenarien ab, die eine Massenmigration der Datenbank von lokalen oder IaaS-Datenbankimplementierungen erfordern. Verwaltete Instanzen unterstützen mehrere Optionen zur Datenbankmigration:

### <a name="back-up-and-restore"></a>Sichern und Wiederherstellen  

Beim Migrationsansatz werden SQL-Sicherungen in Azure Blob Storage genutzt. In Azure Blob Storage gespeicherte Sicherungen können mithilfe des [T-SQL RESTORE-Befehls](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql?view=azuresqldb-mi-current) direkt in der verwalteten Instanz gespeichert werden.

- Eine Schnellstartanleitung zum Wiederherstellen der Standardsicherungsdatei für die Wide World Importers-Datenbank finden Sie unter [Wiederherstellen einer Datenbanksicherung in einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-get-started-restore.md). In diesem Schnellstart wird gezeigt, wie eine Sicherungsdatei in den Azure Blob Storage hochgeladen und mit einem SAS-Schlüssel (Shared Access Signature) geschützt wird.
- Weitere Informationen zur Wiederherstellung über eine URL finden Sie unter [Native RESTORE FROM URL-Option](sql-database-managed-instance-migrate.md#native-restore-from-url).

> [!IMPORTANT]
> Sicherungen von einer verwalteten Instanz können nur in einer anderen verwalteten Instanz wiederhergestellt werden. Sie können nicht auf einem lokalen SQL Server oder in einer Einzeldatenbank/einem Pool für elastische Datenbanken wiederhergestellt werden.

### <a name="data-migration-service"></a>Data Migration Service

Azure Database Migration Service ist ein vollständig verwalteter Dienst, der die nahtlose Migration von mehreren Datenbankquellen zu Azure-Datenplattformen mit minimaler Downtime ermöglicht. Dieser Dienst optimiert die Aufgaben, die erforderlich sind, um bestehende Drittanbieter- und SQL Server-Datenbanken nach Azure SQL-Datenbank (Einzeldatenbanken, Pooldatenbanken in Pools für elastische Datenbanken und Instanzdatenbanken in einer verwalteten Instanz) und SQL Server auf einer Azure-VM zu verschieben. Siehe [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](https://aka.ms/migratetoMIusingDMS).

## <a name="sql-features-supported"></a>Unterstützte SQL-Features

Die Bereitstellungsoption für die verwaltete Instanz strebt für verwaltete Instanzen in mehreren Stufen eine nahezu 100%ige Oberflächenkompatibilität mit der lokalen SQL Server-Version an. Eine Liste der Funktionen und einen Funktionsvergleich finden Sie unter [Funktionsvergleich: Azure SQL-Datenbank und SQL Server](sql-database-features.md) und eine Liste der T-SQL-Unterschiede in verwalteten Instanzen im Vergleich zu SQL Server finden Sie unter [T-SQL-Unterschiede zwischen einer verwalteten Azure SQL-Datenbank-Instanz und SQL Server](sql-database-managed-instance-transact-sql-information.md).

Die Bereitstellungsoption für die verwaltete Instanz unterstützt die Abwärtskompatibilität mit SQL 2008-Datenbanken. Die direkte Migration von SQL 2005-Datenbankservern wird unterstützt, der Kompatibilitätsgrad für migrierte SQL 2005-Datenbanken wird auf SQL 2008 aktualisiert.
  
In der folgenden Abbildung ist die Oberflächenkompatibilität in einer verwalteten Instanz aufgeführt:  

![Migration](./media/sql-database-managed-instance/migration.png)

### <a name="key-differences-between-sql-server-on-premises-and-in-a-managed-instance"></a>Hauptunterschiede zwischen einer lokalen SQL Server-Instanz und einer verwalteten Instanz

Die Bereitstellungsoption für die verwaltete Instanz ist in der Cloud immer auf dem neuesten Stand, d.h., einige Features in der lokalen SQL Server-Instanz sind möglicherweise entweder veraltet oder außer Kraft gesetzt oder weisen Alternativen auf. In spezifischen Fällen müssen Tools erkennen, dass ein bestimmtes Feature auf leicht abweichende Weise ausgeführt wird oder dass ein Dienst in einer Umgebung nicht ausgeführt wird, die Sie nicht vollständig steuern können:

- Hochverfügbarkeit ist anhand einer ähnlichen Technologie wie [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) integriert und vorkonfiguriert.
- Automatisierte Sicherungen und Point-in-Time-Wiederherstellung. Kunden können `copy-only`-Sicherungen initiieren, die die automatische Sicherungskette nicht beeinträchtigen.
- In einer verwalteten Instanz können keine vollständigen physischen Pfade angegeben werden. Daher müssen alle entsprechenden Szenarien unterschiedlich unterstützt werden: RESTORE DB unterstützt WITH MOVE nicht; CREATE DB lässt keine physischen Pfade zu; BULK INSERT kann nur mit Azure-Blobs ausgeführt werden usw.
- Eine verwaltete Instanz unterstützt die [Azure AD-Authentifizierung](sql-database-aad-authentication.md) als Cloudalternative zur Windows-Authentifizierung.
- Eine verwaltete Instanz verwaltet automatisch XTP-Dateigruppen und -Dateien für Datenbanken mit In-Memory-OLTP-Objekten.
- Eine verwaltete Instanz unterstützt SQL Server Integration Services (SSIS) und kann den SSIS-Katalog (SSISDB) hosten, in dem SSIS-Pakete gespeichert werden, sie wird jedoch in einer verwalteten Azure SSIS Integration Runtime (IR) in Azure Data Factory (ADF) ausgeführt. Siehe dazu [Erstellen der Azure SSIS Integration Runtime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). Informationen zum Vergleich der SSIS-Features in SQL-Datenbank finden Sie unter [Vergleich zwischen einer Azure SQL-Datenbank-Einzeldatenbank, einem Pool für elastische Datenbanken und einer verwalteten Instanz](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-a-sql-database-single-database-elastic-pool-and-managed-instance).

### <a name="managed-instance-administration-features"></a>Features zur Verwaltung einer verwalteten Instanz

Dank der Bereitstellungsoption für die verwaltete Instanz muss ein Systemadministrator weniger Zeit für Verwaltungsaufgaben aufwenden, da der SQL-Datenbank-Dienst diese für ihn ausführt oder diese Aufgaben vereinfacht. Beispiele: [Installation und Patches des Betriebssystems und des RDBMS](sql-database-high-availability.md), [dynamische Größenänderung und Konfiguration von Instanzen](sql-database-single-database-scale.md), [Sicherungen](sql-database-automated-backups.md), [Datenbankreplikation](replication-with-sql-database-managed-instance.md) (einschließlich Systemdatenbanken), [Konfiguration der Hochverfügbarkeit](sql-database-high-availability.md) und der Integrität und [Leistungsüberwachung](../azure-monitor/insights/azure-sql.md) der Datenströme.

> [!IMPORTANT]
> Eine Liste der unterstützten, teilweise unterstützten und nicht unterstützten Features finden Sie unter [Features von Azure SQL-Datenbank](sql-database-features.md). Eine Liste der T-SQL-Unterschiede in verwalteten Instanzen im Vergleich zu SQL Server finden Sie unter [T-SQL-Unterschiede zwischen einer verwalteten Azure SQL-Datenbank-Instanz und SQL Server](sql-database-managed-instance-transact-sql-information.md).

### <a name="how-to-programmatically-identify-a-managed-instance"></a>Programmgesteuertes Identifizieren einer verwalteten Instanz

In der folgenden Tabelle sind verschiedene Eigenschaften aufgeführt, auf die über Transact-SQL zugegriffen werden kann und über die Sie ermitteln können, ob Ihre Anwendung mit der verwalteten Instanz ausgeführt wird, und mit denen Sie wichtige Eigenschaften abrufen können.

|Eigenschaft|Wert|Comment|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 2018-03-07 Copyright (C) 2018 Microsoft Corporation|Dieser Wert ist mit dem Wert in SQL-Datenbank identisch.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Dieser Wert ist mit dem Wert in SQL-Datenbank identisch.|
|`SERVERPROPERTY('EngineEdition')`|8|Durch diesen Wert wird eine verwaltete Instanz eindeutig identifiziert.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Vollständiger DNS-Instanzname im folgenden Format: `<instanceName>`.`<dnsPrefix>`.database.windows.net, wobei `<instanceName>` der vom Kunden angegebene Name und `<dnsPrefix>` der automatisch generierte Teil des Namens ist, der die Eindeutigkeit des globalen DNS-Namens gewährleistet (z.B. „wcus17662feb9ce98“)|Beispiel: meine-verwaltete-instanz.wcus17662feb9ce98.database.windows.net|

## <a name="next-steps"></a>Nächste Schritte

- Im [Schnellstarthandbuch](sql-database-managed-instance-get-started.md) erfahren Sie, wie Sie Ihre erste verwaltete Instanz erstellen.
- Eine Liste der Features und einen Funktionsvergleich finden Sie unter [Allgemeine SQL-Features](sql-database-features.md).
- Weitere Informationen zur VNET-Konfiguration finden Sie unter [Konfigurieren eines VNET für eine verwaltete Azure SQL-Datenbank-Instanz](sql-database-managed-instance-connectivity-architecture.md).
- Eine Schnellstartanleitung, in der eine verwaltete Instanz erstellt und eine Datenbank von einer Sicherungsdatei wiederhergestellt wird, finden Sie unter [Erstellen einer verwalteten Azure SQL-Datenbank-Instanz](sql-database-managed-instance-get-started.md).
- Ein Tutorial mit Verwendung des Azure Database Migration Service (DMS) für die Migration finden Sie unter [Migrieren von SQL Server zu einer verwalteten Azure SQL-Datenbank-Instanz](../dms/tutorial-sql-server-to-managed-instance.md).
- Informationen zur erweiterten Überwachung der Datenbankleistung verwalteter Instanzen mit integrierten Problembehandlungsfunktionen finden Sie unter [Überwachen von Azure SQL-Datenbank mithilfe von Azure SQL-Analyse (Vorschauversion)](../azure-monitor/insights/azure-sql.md).
- Preisinformationen finden Sie unter [Preise – verwaltete Azure SQL-Datenbank-Instanzen ](https://azure.microsoft.com/pricing/details/sql-database/managed/).
