---
title: Tarif „Unternehmenskritisch“ – Azure SQL-Datenbank-Dienst | Microsoft-Dokumentation
description: Informationen zum Tarif „Unternehmenskritisch“ für Azure SQL-Datenbank
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 12/04/2018
ms.openlocfilehash: ef1f420e4c4dbd38ad703eb0573fae36af078edb
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73496238"
---
# <a name="business-critical-tier---azure-sql-database"></a>Tarif „Unternehmenskritisch“ – Azure SQL-Datenbank

> [!NOTE]
> Der Tarif „Unternehmenskritisch“ hat im DTU-Kaufmodell den Namen „Premium“. Einen Vergleich zwischen vCore-basiertem Kaufmodell und DTU-basiertem Kaufmodell finden Sie unter [Kaufmodelle für Azure SQL-Datenbank und Ressourcen](sql-database-purchase-models.md).

Azure SQL-Datenbank basiert auf der an die Cloudumgebung angepasste Architektur der SQL Server-Datenbank-Engine, um die Verfügbarkeit von 99,99 % selbst bei Infrastrukturausfällen sicherzustellen. In Azure SQL-Datenbank werden drei Architekturmodelle verwendet:
- Universell/Standard 
- Unternehmenskritisch/Premium
- Hyperscale

Das Dienstebenenmodell des Typs „Premium/Unternehmenskritisch“ basiert auf einem Cluster von Datenbank-Engine-Prozessen. Dieses Architekturmodell beruht darauf, dass immer ein Quorum von verfügbaren Datenbank-Engine-Knoten vorhanden ist, und weist selbst während Wartungsaktivitäten eine minimale Leistungsbeeinträchtigung der Workload auf.

In Azure werden das zugrunde liegende Betriebssystem, Treiber und die SQL Server-Datenbank-Engine transparent mit minimaler Ausfallzeit für Endbenutzer aktualisiert und gepatcht. 

Die Premium-Verfügbarkeit wird in den Dienstebenen „Premium“ und „Unternehmenskritisch“ von Azure SQL-Datenbank aktiviert und ist für hohe Workloads vorgesehen, bei denen keine Leistungsbeeinträchtigung aufgrund laufender Wartungsvorgänge toleriert werden kann.

Im Premium-Modell werden in Azure SQL-Datenbank der Compute- und Speicherbereich auf dem einzelnen Knoten integriert. Die Hochverfügbarkeit in diesem Architekturmodell wird durch die Replikation auf Compute- (SQL Server-Datenbank-Engine-Prozess) und Speicherebene (lokal angefügte SSD) erreicht, die in einem Cluster mit vier Knoten anhand einer ähnlichen Technologie wie [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) in SQL Server bereitgestellt werden.

![Cluster von Datenbank-Engine-Knoten](media/sql-database-managed-instance/business-critical-service-tier.png)

Der SQL-Datenbank-Engine-Prozess sowie die zugrunde liegenden MDF- und LDF-Dateien werden mit lokal angefügtem SSD-Speicher auf demselben Knoten platziert und bieten eine geringe Latenz für die Workload. Hochverfügbarkeit wird anhand einer ähnlichen Technologie wie [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) in SQL Server implementiert. Jede Datenbank ist ein Cluster von Datenbankknoten mit einer primären Datenbank, die für die Kundenworkload zugänglich ist, und drei sekundären Prozessen, die Kopien der Daten enthalten. Der primäre Knoten überträgt die Änderungen kontinuierlich zu sekundären Knoten, um sicherzustellen, dass die Daten auf sekundären Replikaten verfügbar sind, wenn der primäre Knoten aus bestimmten Gründen ausfällt. Ein Failover wird von der SQL Server-Datenbank-Engine verarbeitet: Ein sekundäres Replikat wird zum primären Knoten, und ein neues sekundäres Replikat wird erstellt, um sicherzustellen, dass ausreichend Knoten im Cluster vorhanden sind. Die Workload wird automatisch zum neuen primären Knoten umgeleitet.

Darüber hinaus umfasst der Cluster des Typs „Unternehmenskritisch“ eine integrierte Funktion [Horizontale Leseskalierung](sql-database-read-scale-out.md), die einen gebührenfreien, integrierten und schreibgeschützten Knoten bereitstellt, der zum Ausführen von schreibgeschützten Abfragen (z.B. Berichten) verwendet werden kann, die die Leistung der primären Workload nicht beeinträchtigen sollen.

## <a name="when-to-choose-this-service-tier"></a>Wann sollte diese Dienstebene gewählt werden?

Die Dienstebene „Unternehmenskritisch“ ist für die Anwendungen gedacht, die Antworten mit geringer Latenz vom zugrunde liegenden SSD-Speicher (durchschnittlich 1 – 2 ms), schnelle Wiederherstellung bei einem Fehler der zugrunde liegenden Infrastruktur oder das Auslagern von Berichten, Analysen und schreibgeschützten Abfragen an das kostenlose lesbare sekundäre Replikat der primären Datenbank erfordern.

Die wichtigsten Gründe dafür, dass Sie die Dienstebene „Unternehmenskritisch“ anstelle der Ebene „Universell“ wählen sollten, sind folgende:
-   Niedrige E/A-Latenzanforderungen: Workloads, die eine schnelle Antwort von der Speicherebene benötigen (durchschnittlich 1–2 Millisekunden), sollten die Ebene „Unternehmenskritisch“ verwenden. 
-   Häufige Kommunikation zwischen der Anwendung und der Datenbank. Anwendungen, die eine Zwischenspeicherung auf Anwendungsebene oder die [Batchverarbeitung von Anforderungen](sql-database-use-batching-to-improve-performance.md) nicht nutzen können und viele SQL-Abfragen senden müssen, für die eine schnelle Verarbeitung erforderlich ist, sind gute Kandidaten für die Ebene „Unternehmenskritisch“.
-   Eine große Anzahl von Aktualisierungen: Einfüge-, Aktualisierungs- und Löschvorgänge verändern die Datenseiten im Arbeitsspeicher (modifizierte Seite), die mit dem Vorgang `CHECKPOINT` in Datendateien gespeichert werden müssen. Ein potenzieller Prozessabsturz der Datenbank-Engine oder ein Failover der Datenbank mit einer großen Anzahl von modifizierten Seiten erhöht unter Umständen die Wiederherstellungszeit in der Ebene „Universell“. Verwenden Sie die Ebene „Unternehmenskritisch“, wenn Sie über eine Workload verfügen, die zu vielen In-Memory-Änderungen führt. 
-   Zeitintensive Transaktionen, die Daten ändern. Transaktionen, die für einen längeren Zeitraum geöffnet sind, verhindern das Abschneiden von Protokolldateien, was die Protokollgröße und die Anzahl von [virtuellen Protokolldateien](https://docs.microsoft.com/sql/relational-databases/sql-server-transaction-log-architecture-and-management-guide#physical_arch) (virtual log file, VLF) erhöhen kann. Eine hohe Anzahl von VLF kann die Wiederherstellung der Datenbank nach einem Failover verlangsamen.
-   Eine Workload mit Berichterstellungs- und Analyseabfragen, die zum kostenlosen sekundären schreibgeschützten Replikat umgeleitet werden können.
- Höhere Resilienz und schnellere Wiederherstellung nach Fehlern. Bei einem Systemausfall wird die Datenbank auf der primären Instanz deaktiviert, und eines der sekundären Replikate wird sofort zur neuen primären Datenbank mit Lese-/Schreibzugriff, die die Abfragen verarbeiten kann. Es ist nicht erforderlich, dass die Datenbank-Engine Transaktionen aus der Protokolldatei analysiert und wiederholt und alle Daten im Speicherpuffer lädt.
- Erweiterter Schutz vor Datenbeschädigung: Die Ebene „Unternehmenskritisch“ nutzt im Hintergrund Datenbankreplikate, um die Geschäftskontinuität sicherzustellen, und der Dienst nutzt auch die automatische Seitenreparatur. Dabei handelt es sich um dieselbe Technologie, die für die SQL Server-[Datenbankspiegelung und Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring) genutzt wird. Wenn ein Replikat eine Seite aufgrund eines Datenintegritätsproblems nicht lesen kann, wird eine neue Kopie der Seite von einem anderen Replikat abgerufen, die die nicht lesbare Seite ohne Datenverluste oder Ausfallzeiten für den Kunden ersetzt. Diese Funktionalität ist in der Ebene „Universell“ verfügbar, wenn die Datenbank über ein georedundantes Replikat verfügt.
- Höhere Verfügbarkeit: Die Ebene „Unternehmenskritisch“ in einer Konfiguration mit mehreren Verfügbarkeitszonen garantiert eine Verfügbarkeit von 99,995 %, die Ebene „Universell“ im Vergleich dazu 99,99 %.
- Schnelle geografische Wiederherstellung: Die mit Georeplikation konfigurierte Ebene „Unternehmenskritisch“ bietet eine garantierte Recovery Point Objective (RPO) von fünf Sekunden und eine Recovery Time Objective (RTO) von 30 Sekunden für 100 % der bereitgestellten Stunden.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Ressourcenmerkmale (Anzahl von Kernen, E/A, Arbeitsspeicher) der Ebene „Unternehmenskritisch“ für eine [verwaltete Instanz](sql-database-managed-instance-resource-limits.md#service-tier-characteristics), für eine Einzeldatenbank im [V-Kern-Modell](sql-database-vcore-resource-limits-single-databases.md#business-critical---provisioned-compute---gen4) bzw. [DTU-Modell](sql-database-dtu-resource-limits-single-databases.md#premium-service-tier) oder für einen Pool für elastische Datenbanken im [V-Kern-Modell](sql-database-vcore-resource-limits-elastic-pools.md#business-critical---provisioned-compute---gen4) und [DTU-Modell](sql-database-dtu-resource-limits-elastic-pools.md#premium-elastic-pool-limits) an.
- Erfahren Sie mehr über die Tarife [Universell](sql-database-service-tier-general-purpose.md) und [Hyperscale](sql-database-service-tier-hyperscale.md).
- Erfahren Sie mehr über [Service Fabric](../service-fabric/service-fabric-overview.md).
- Weitere Optionen zu Hochverfügbarkeit und Notfallwiederherstellung finden Sie unter [Geschäftskontinuität](sql-database-business-continuity.md).
