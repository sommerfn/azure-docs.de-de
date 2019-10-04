---
title: Hochverfügbarkeit – Azure SQL-Datenbank-Dienst | Microsoft-Dokumentation
description: Erfahren Sie mehr über die Hochverfügbarkeitsfunktionen des Azure SQL-Datenbank-Diensts.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: sashan
ms.reviewer: carlrab, sashan
ms.date: 06/10/2019
ms.openlocfilehash: 226b0c1cb11fc872cb7759e0d0e49275b9c2d9bf
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68568143"
---
# <a name="high-availability-and-azure-sql-database"></a>Hochverfügbarkeit und Azure SQL-Datenbank

Die Architektur für Hochverfügbarkeit in Azure SQL-Datenbank soll garantieren, dass Ihre Datenbank aktiv ist und zu 99,99 % der Zeit ausgeführt wird, ohne dass Sie sich Gedanken über Auswirkungen von Wartungsarbeiten und Ausfällen machen müssen. Azure verarbeitet automatisch wichtige Wartungsaufgaben, z.B. Patches, Sicherungen, Windows- und SQL-Upgrades, aber auch ungeplante Ereignisse wie Ausfälle der zugrunde liegenden Hardware oder Software oder Netzwerkfehler.  Wenn die zugrunde liegende SQL-Instanz gepatcht oder ein Failover für die SQL-Instanz ausgeführt wird, ist im Allgemeinen keine Ausfallzeit festzustellen, wenn Sie in Ihrer App [Wiederholungslogik nutzen](sql-database-develop-overview.md#resiliency). Azure SQL-Datenbank kann auch unter den kritischsten Umständen schnell wiederhergestellt werden. So wird sichergestellt, dass Ihre Daten immer verfügbar sind.

Die Hochverfügbarkeitslösung soll sicherstellen, dass Daten, für die ein Commit ausgeführt wurde, nie aufgrund von Fehlern verloren gehen, dass sich Wartungsvorgänge nicht auf Ihre Workload auswirken und dass die Datenbank keinen Single Point of Failure in der Softwarearchitektur darstellt. Es gibt keine Wartungsfenster oder Ausfallzeiten, aufgrund derer Sie die Workload während der Aktualisierung oder Wartung der Datenbank beenden müssen. 

In Azure SQL-Datenbank werden zwei Hochverfügbarkeits-Architekturmodelle verwendet:

- Das Standardverfügbarkeitsmodell, das auf der Trennung der Compute- und Speicherebene basiert.  Es basiert auf der Hochverfügbarkeit und der Zuverlässigkeit der Remotespeicherebene. Diese Architektur ist auf budgetgebundene Geschäftsanwendungen ausgelegt, die bei Wartungsarbeiten gewisse Leistungseinbußen tolerieren können.
- Das Premium-Verfügbarkeitsmodell, das auf einem Cluster von Datenbank-Engine-Prozessen basiert. Dieses beruht auf dem Umstand, dass stets ein Quorum von verfügbaren Datenbank-Engine-Knoten vorhanden ist. Diese Architektur ist auf unternehmenskritische Anwendungen mit hoher E/A-Leistung und einer hohen Transaktionsrate ausgelegt; es garantiert während Wartungsaktivitäten minimale Leistungseinbußen für Ihre Workload.

Azure SQL-Datenbank wird auf der aktuellen stabilen Version der SQL Server-Datenbank-Engine und des Windows-Betriebssystems ausgeführt. Die meisten Benutzer bemerken nicht, dass laufend Upgrades ausgeführt werden.

## <a name="basic-standard-and-general-purpose-service-tier-availability"></a>Verfügbarkeit der Dienstebenen „Basic“, „Standard“ und „Universell“

Diese Dienstebenen nutzen die Standardverfügbarkeitsarchitektur. In der folgenden Abbildung werden vier Knoten mit getrennter Compute- und Speicherebene veranschaulicht.

![Trennung der Compute- und Speicherebene](media/sql-database-high-availability/general-purpose-service-tier.png)

Das Standardverfügbarkeitsmodell umfasst zwei Ebenen:

- Eine zustandslose Compute-Ebene, auf der der Prozess `sqlserver.exe` ausgeführt wird und die auf der angehängten SSD nur temporäre und zwischengespeicherte Daten enthält (z.B. TempDB, Modelldatenbank, Plancache, Pufferpool und Spaltenspeicherpool). Dieser zustandslose Knoten wird von Azure Service Fabric gesteuert, die `sqlserver.exe` initialisiert, die Integrität des Knotens steuert und bei Bedarf ein Failover zu einem anderen Knoten durchführt.
- Eine zustandsbehaftete Datenebene mit den Datenbankdateien (MDF- und LDF-Dateien), die in Azure Blob Storage gespeichert sind. Azure Blob Storage verfügt über integrierte Datenverfügbarkeits- und Redundanzfunktionen. Dadurch wird sichergestellt, dass jeder Datensatz in der Protokolldatei bzw. jede Seite in der Datendatei erhalten bleibt, auch wenn der SQL Server-Prozess abstürzt.

Bei jedem Upgrade der Datenbank-Engine oder des Betriebssystems sowie beim Erkennen eines Fehlers wird der zustandslose SQL Server-Prozess in Azure Service Fabric zu einem anderen zustandslosen Computeknoten mit ausreichender freier Kapazität verschoben. Daten in Azure Blob Storage sind vom Verschiebevorgang nicht betroffen, und die Daten- und Protokolldateien werden an den neu initialisierten SQL Server-Prozess angefügt. Dieser Prozess garantiert eine Verfügbarkeit von 99,99 %; bei einer starken Workload ist möglicherweise eine gewisse Leistungseinbuße während des Übergangs festzustellen, da die neue SQL Server-Instanz mit einem kalten Cache gestartet wird.

## <a name="premium-and-business-critical-service-tier-availability"></a>Verfügbarkeit der Dienstebenen „Premium“ und „Unternehmenskritisch“

Die Dienstebenen „Premium“ und „Unternehmenskritisch“ nutzen das Premium-Verfügbarkeitsmodell, das eine Integration von Computeressourcen (SQL Server-Datenbank-Engine-Prozess) und Speicher (lokal angefügte SSD) auf einem einzigen Knoten bietet. Hochverfügbarkeit wird durch Replizieren von Compute- und Speicherressourcen auf weiteren Knoten erreicht, wodurch ein Cluster mit drei bis vier Knoten erstellt wird. 

![Cluster von Datenbank-Engine-Knoten](media/sql-database-high-availability/business-critical-service-tier.png)

Die zugrunde liegenden Datenbankdateien (MDF- und LDF-Dateien) werden auf dem angefügten SSD-Speicher platziert, um eine E/A mit äußerst niedriger Latenz für Ihre Workload zu erzielen. Hochverfügbarkeit wird anhand einer ähnlichen Technologie wie [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) in SQL Server implementiert. Der Cluster umfasst ein einzelnes primäres Replikat (SQL Server-Prozess), der für Lese-/Schreib-Workloads der Kunden zugänglich ist, sowie bis zu drei sekundäre Replikate (Compute und Speicher) mit Kopien der Daten. Der primäre Knoten ständig überträgt Änderungen der Reihe nach auf die sekundären Knoten und stellt sicher, dass die Daten vor dem Ausführen eines Commits für jede Transaktion mit mindestens einem sekundären Replikat synchronisiert werden. Durch diesen Prozess wird sichergestellt, dass bei einem Ausfall des primären Knotens stets ein vollständig synchronisierter Knoten vorhanden ist, auf den ein Failover ausgeführt werden kann. Das Failover wird von der Azure Service Fabric initiiert. Sobald das sekundäre Replikat zum neuen primären Knoten wird, wird ein weiteres sekundäres Replikat erstellt, um sicherzustellen, dass der Cluster über eine ausreichende Anzahl von Knoten (Quorumssatz) verfügt. Nach Abschluss des Failovers werden SQL-Verbindungen automatisch an den neuen primären Knoten umgeleitet.

Als weiteren Vorteil bietet das Premium-Verfügbarkeitsmodell die Möglichkeit, SQL-Verbindungen mit Schreibschutz auf eines der sekundären Replikate umzuleiten. Dieses Feature wird als [horizontale Leseskalierung](sql-database-read-scale-out.md) bezeichnet. Es bietet 100 % zusätzliche Computekapazität ohne anfallende Zusatzkosten, sodass Schreibschutzvorgänge wie analytische Workloads vom primären Replikat ausgelagert werden können.

## <a name="zone-redundant-configuration"></a>Zonenredundante Konfiguration

In der Standardeinstellung wird der Cluster von Knoten für das Premium-Verfügbarkeitsmodell im selben Rechenzentrum erstellt. Mit der Einführung von [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md) kann Azure SQL-Datenbank nun verschiedene Replikate im Cluster in unterschiedlichen Verfügbarkeitszonen in derselben Region platzieren. Um einen Single Point of Failure auszuschließen, wird der Steuerring zudem in mehreren Zonen als drei Gatewayringe (GW) kopiert. Die Weiterleitung an einen bestimmten Gatewayring wird durch [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM) gesteuert. Da bei der zonenredundanten Konfiguration in den Dienstebenen „Premium“ oder „Unternehmenskritisch“ keine zusätzliche Datenbankredundanz erzeugt wird, können Sie sie ohne Zusatzkosten aktivieren. Durch die Auswahl einer zonenredundanten Konfiguration können Sie Ihre Datenbanken der Dienstebenen „Premium“ oder „Unternehmenskritisch“ für deutlich mehr Ausfallszenarien resistent machen (z.B. für schwerwiegende Ausfälle von Rechenzentren), ohne Änderungen an der Anwendungslogik vornehmen zu müssen. Sie können zudem alle vorhandenen Datenbanken oder Pools der Dienstebenen „Premium“ oder „Unternehmenskritisch“ in die zonenredundante Konfiguration konvertieren.

Da die zonenredundanten Datenbanken über Replikate in verschiedenen Rechenzentren mit einiger Entfernung dazwischen verfügen, kann sich durch die erhöhte Netzwerklatenz die Commitzeit erhöhen und dadurch die Leistung einiger OLTP-Workloads beeinträchtigt werden. Sie können jederzeit zur Einzelzonenkonfiguration zurückkehren, indem Sie die zonenredundante Einstellung deaktivieren. Dieser Prozess ist ein Onlinevorgang und ähnelt dem regulären Dienstebenen-Upgrade. Am Ende des Prozesses wird die Datenbank oder der Pool aus einem zonenredundanten Ring zum Ring einer einzelnen Zone migriert (oder umgekehrt).

> [!IMPORTANT]
> Zonenredundante Datenbanken und Pools für elastische Datenbanken werden derzeit nur auf den Dienstebenen „Premium“ und „Unternehmenskritisch“ in ausgewählten Regionen unterstützt. Bei Verwendung des Tarifs „Unternehmenskritisch“ ist die zonenredundante Konfiguration nur verfügbar, wenn die Gen5-Computehardware ausgewählt ist. Aktuelle Informationen über die Regionen, die zonenredundante Datenbanken unterstützen, finden Sie unter [Unterstützung der Dienste nach Region](../availability-zones/az-overview.md#services-support-by-region).  

Die zonenredundante Version der Hochverfügbarkeitsarchitektur wird im folgenden Diagramm veranschaulicht:

![Hochverfügbarkeitsarchitektur, zonenredundant](./media/sql-database-high-availability/zone-redundant-business-critical-service-tier.png)

## <a name="accelerated-database-recovery-adr"></a>Schnellere Datenbankwiederherstellung

Die [schnellere Datenbankwiederherstellung (Accelerated Database Recovery, ADR)](sql-database-accelerated-database-recovery.md) ist eine neue Funktion der SQL-Datenbank-Engine. Mit dieser Funktion wird die Datenbankverfügbarkeit erheblich verbessert, insbesondere bei Transaktionen mit langer Ausführungsdauer. ADR ist derzeit für Einzeldatenbanken, Pools für elastische Datenbanken und Azure SQL Data Warehouse verfügbar.

## <a name="conclusion"></a>Zusammenfassung

Azure SQL-Datenbank verfügt über eine integrierte Hochverfügbarkeitslösung, die tief in die Azure-Plattform integriert ist. Sie ist bei der Fehlererkennung und Wiederherstellung von Service Fabric, in Verbindung mit dem Datenschutz von Azure Blob Storage und für höhere Fehlertoleranz von Verfügbarkeitszonen abhängig. Darüber hinaus nutzt Azure SQL-Datenbank die Technologie der AlwaysOn-Verfügbarkeitsgruppen von SQL Server für Replikation und Failover. Dank der Kombination dieser Technologien können Anwendungen die Vorteile eines gemischten Speichermodells voll ausschöpfen und sehr anspruchsvolle SLAs unterstützen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md)
- Weitere Informationen zu [Service Fabric](../service-fabric/service-fabric-overview.md)
- Weitere Informationen zu [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
- Weitere Optionen für Hochverfügbarkeit und Notfallwiederherstellung finden Sie unter [Geschäftskontinuität](sql-database-business-continuity.md).
