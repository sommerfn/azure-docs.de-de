---
title: Hochverfügbarkeit
description: Erfahren Sie mehr über die Hochverfügbarkeitsfunktionen des Azure SQL-Datenbank-Diensts.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: sashan
ms.author: sashan
ms.reviewer: carlrab, sashan
ms.date: 10/14/2019
ms.openlocfilehash: 86a3fd7c67dc2e544a1510dc910951452c32245d
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73811349"
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

- Eine zustandslose Compute-Ebene, auf der der Prozess `sqlservr.exe` ausgeführt wird und die nur vorübergehende und zwischengespeicherte Daten enthält, z. B. TempDB, Modelldatenbanken auf der angefügten SSD, Plancache, Puffer- und Columnstore-Pool im Arbeitsspeicher. Dieser zustandslose Knoten wird von Azure Service Fabric gesteuert, die `sqlservr.exe` initialisiert, die Integrität des Knotens steuert und bei Bedarf ein Failover zu einem anderen Knoten durchführt.
- Eine zustandsbehaftete Datenebene mit den Datenbankdateien (MDF- und LDF-Dateien), die in Azure Blob Storage gespeichert sind. Azure Blob Storage verfügt über integrierte Datenverfügbarkeits- und Redundanzfunktionen. Dadurch wird sichergestellt, dass jeder Datensatz in der Protokolldatei bzw. jede Seite in der Datendatei erhalten bleibt, auch wenn der SQL Server-Prozess abstürzt.

Bei jedem Upgrade der Datenbank-Engine oder des Betriebssystems sowie beim Erkennen eines Fehlers wird der zustandslose SQL Server-Prozess in Azure Service Fabric zu einem anderen zustandslosen Computeknoten mit ausreichender freier Kapazität verschoben. Daten in Azure Blob Storage sind vom Verschiebevorgang nicht betroffen, und die Daten- und Protokolldateien werden an den neu initialisierten SQL Server-Prozess angefügt. Dieser Prozess garantiert eine Verfügbarkeit von 99,99 %; bei einer starken Workload ist möglicherweise eine gewisse Leistungseinbuße während des Übergangs festzustellen, da die neue SQL Server-Instanz mit einem kalten Cache gestartet wird.

## <a name="premium-and-business-critical-service-tier-availability"></a>Verfügbarkeit der Dienstebenen „Premium“ und „Unternehmenskritisch“

Die Dienstebenen „Premium“ und „Unternehmenskritisch“ nutzen das Premium-Verfügbarkeitsmodell, das eine Integration von Computeressourcen (SQL Server-Datenbank-Engine-Prozess) und Speicher (lokal angefügte SSD) auf einem einzigen Knoten bietet. Hochverfügbarkeit wird durch Replizieren von Compute- und Speicherressourcen auf weiteren Knoten erreicht, wodurch ein Cluster mit drei bis vier Knoten erstellt wird. 

![Cluster von Datenbank-Engine-Knoten](media/sql-database-high-availability/business-critical-service-tier.png)

Die zugrunde liegenden Datenbankdateien (MDF- und LDF-Dateien) werden auf dem angefügten SSD-Speicher platziert, um eine E/A mit äußerst niedriger Latenz für Ihre Workload zu erzielen. Hochverfügbarkeit wird anhand einer ähnlichen Technologie wie [AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server) in SQL Server implementiert. Der Cluster umfasst ein einzelnes primäres Replikat (SQL Server-Prozess), der für Lese-/Schreib-Workloads der Kunden zugänglich ist, sowie bis zu drei sekundäre Replikate (Compute und Speicher) mit Kopien der Daten. Der primäre Knoten ständig überträgt Änderungen der Reihe nach auf die sekundären Knoten und stellt sicher, dass die Daten vor dem Ausführen eines Commits für jede Transaktion mit mindestens einem sekundären Replikat synchronisiert werden. Durch diesen Prozess wird sichergestellt, dass bei einem Ausfall des primären Knotens stets ein vollständig synchronisierter Knoten vorhanden ist, auf den ein Failover ausgeführt werden kann. Das Failover wird von der Azure Service Fabric initiiert. Sobald das sekundäre Replikat zum neuen primären Knoten wird, wird ein weiteres sekundäres Replikat erstellt, um sicherzustellen, dass der Cluster über eine ausreichende Anzahl von Knoten (Quorumssatz) verfügt. Nach Abschluss des Failovers werden SQL-Verbindungen automatisch an den neuen primären Knoten umgeleitet.

Als weiteren Vorteil bietet das Premium-Verfügbarkeitsmodell die Möglichkeit, SQL-Verbindungen mit Schreibschutz auf eines der sekundären Replikate umzuleiten. Dieses Feature wird als [horizontale Leseskalierung](sql-database-read-scale-out.md) bezeichnet. Es bietet 100 % zusätzliche Computekapazität ohne anfallende Zusatzkosten, sodass Schreibschutzvorgänge wie analytische Workloads vom primären Replikat ausgelagert werden können.

## <a name="hyperscale-service-tier-availability"></a>Verfügbarkeit der Dienstebene „Hyperscale“

Die Architektur der Dienstebene „Hyperscale“ wird unter [Architektur mit verteilten Funktionen](https://docs.microsoft.com/azure/sql-database/sql-database-service-tier-hyperscale#distributed-functions-architecture) beschrieben. 

![Funktionale Hyperscale-Architektur](./media/sql-database-hyperscale/hyperscale-architecture.png)

Das Verfügbarkeitsmodell in Hyperscale umfasst vier Ebenen:

- Eine zustandslose Compute-Ebene, auf der der Prozess `sqlservr.exe` ausgeführt wird und die nur vorübergehende und zwischengespeicherte Daten enthält, z. B. nicht abdeckenden RBPEX-Cache TempDB, Modelldatenbanken usw. auf der angefügten SSD, Plancache, Puffer- und Columnstore-Pool im Arbeitsspeicher. Diese zustandslose Ebene enthält das primäre Computereplikat und optional eine Reihe sekundärer Computereplikate, die als Failoverziele dienen können.
- Eine durch Seitenserver gebildete zustandslose Speicherebene. Diese Ebene ist das verteilte Speichermodul für die `sqlservr.exe`-Prozesse, die auf den Computereplikaten ausgeführt werden. Jeder Seitenserver enthält nur vorübergehende und zwischengespeicherte Daten, wie z.B. abdeckenden RBPEX-Cache auf der angefügten SSD und im Arbeitsspeicher zwischengespeicherte Datenseiten. Jeder Seitenserver verfügt über einen gekoppelten Seitenserver in einer Aktiv-Aktiv-Konfiguration, um Lastenausgleich, Redundanz und Hochverfügbarkeit zu gewährleisten.
- Eine zustandsbehaftete Speicherschicht für Transaktionsprotokolle, die vom Computeknoten gebildet wird, auf dem der Protokolldienstprozess, die Zielzone für Transaktionsprotokolle und der langfristige Speicher für Transaktionsprotokolle ausgeführt werden. Für die Zielzone und den langfristigen Speicher wird Azure Storage eingesetzt. Dieser Dienst bietet Verfügbarkeit und [Redundanz](https://docs.microsoft.com/azure/storage/common/storage-redundancy) für das Transaktionsprotokoll und gewährleistet Datenbeständigkeit für durchgeführte Transaktionen.
- Eine zustandsbehaftete Datenspeicherebene mit den Datenbankdateien (MDF- und NDF-Dateien), die in Azure Storage gespeichert und von Seitenservern aktualisiert werden. Diese Ebene nutzt die Features von Azure Storage für Datenverfügbarkeit und [Redundanz](https://docs.microsoft.com/azure/storage/common/storage-redundancy). Sie garantiert, dass jede Seite in einer Datendatei erhalten bleibt, auch wenn Prozesse auf anderen Ebenen der Hyperscale-Architektur abstürzen oder Computeknoten ausfallen.

Computeknoten auf allen Hyperscale-Ebenen werden in Azure Service Fabric ausgeführt. Dieser Dienst kontrolliert die Integrität jedes Knotens und führt bei Bedarf ein Failover auf verfügbare fehlerfreie Knoten durch.

Weitere Informationen zur Hochverfügbarkeit in Hyperscale finden Sie unter [Hochverfügbarkeit der Datenbank in Hyperscale](https://docs.microsoft.com/azure/sql-database/sql-database-service-tier-hyperscale#database-high-availability-in-hyperscale).

## <a name="zone-redundant-configuration"></a>Zonenredundante Konfiguration

In der Standardeinstellung wird der Cluster von Knoten für das Premium-Verfügbarkeitsmodell im selben Rechenzentrum erstellt. Mit der Einführung von [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md) kann Azure SQL-Datenbank nun verschiedene Replikate von Datenbanken des Typs „Unternehmenskritisch“ in unterschiedlichen Verfügbarkeitszonen in derselben Region platzieren. Um einen Single Point of Failure auszuschließen, wird der Steuerring zudem in mehreren Zonen als drei Gatewayringe (GW) kopiert. Die Weiterleitung an einen bestimmten Gatewayring wird durch [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) (ATM) gesteuert. Da bei der zonenredundanten Konfiguration in den Dienstebenen „Premium“ oder „Unternehmenskritisch“ keine zusätzliche Datenbankredundanz erzeugt wird, können Sie sie ohne Zusatzkosten aktivieren. Durch die Auswahl einer zonenredundanten Konfiguration können Sie Ihre Datenbanken der Dienstebenen „Premium“ oder „Unternehmenskritisch“ für deutlich mehr Ausfallszenarien resistent machen (z.B. für schwerwiegende Ausfälle von Rechenzentren), ohne Änderungen an der Anwendungslogik vornehmen zu müssen. Sie können zudem alle vorhandenen Datenbanken oder Pools der Dienstebenen „Premium“ oder „Unternehmenskritisch“ in die zonenredundante Konfiguration konvertieren.

Da die zonenredundanten Datenbanken über Replikate in verschiedenen Rechenzentren mit einiger Entfernung dazwischen verfügen, kann sich durch die erhöhte Netzwerklatenz die Commitzeit erhöhen und dadurch die Leistung einiger OLTP-Workloads beeinträchtigt werden. Sie können jederzeit zur Einzelzonenkonfiguration zurückkehren, indem Sie die zonenredundante Einstellung deaktivieren. Dieser Prozess ist ein Onlinevorgang und ähnelt dem regulären Dienstebenen-Upgrade. Am Ende des Prozesses wird die Datenbank oder der Pool aus einem zonenredundanten Ring zum Ring einer einzelnen Zone migriert (oder umgekehrt).

> [!IMPORTANT]
> Zonenredundante Datenbanken und Pools für elastische Datenbanken werden derzeit nur auf den Dienstebenen „Premium“ und „Unternehmenskritisch“ in ausgewählten Regionen unterstützt. Bei Verwendung des Tarifs „Unternehmenskritisch“ ist die zonenredundante Konfiguration nur verfügbar, wenn die Gen5-Computehardware ausgewählt ist. Aktuelle Informationen über die Regionen, die zonenredundante Datenbanken unterstützen, finden Sie unter [Unterstützung der Dienste nach Region](../availability-zones/az-overview.md#services-support-by-region).  
> Dieses Feature steht in einer verwalteten Instanz nicht zur Verfügung.

Die zonenredundante Version der Hochverfügbarkeitsarchitektur wird im folgenden Diagramm veranschaulicht:

![Hochverfügbarkeitsarchitektur, zonenredundant](./media/sql-database-high-availability/zone-redundant-business-critical-service-tier.png)

## <a name="accelerated-database-recovery-adr"></a>Schnellere Datenbankwiederherstellung

Die [schnellere Datenbankwiederherstellung (Accelerated Database Recovery, ADR)](sql-database-accelerated-database-recovery.md) ist eine neue Funktion der SQL-Datenbank-Engine. Mit dieser Funktion wird die Datenbankverfügbarkeit erheblich verbessert, insbesondere bei Transaktionen mit langer Ausführungsdauer. ADR ist derzeit für Einzeldatenbanken, Pools für elastische Datenbanken und Azure SQL Data Warehouse verfügbar.

## <a name="testing-application-fault-resiliency"></a>Testen der Resilienz von Anwendungsfehlern

Hochverfügbarkeit ist ein wesentlicher Bestandteil der Azure SQL-Datenbank-Plattform, der für Ihre Datenbankanwendung transparent ausgeführt wird. Es ist uns jedoch bewusst, dass Sie möglicherweise testen möchten, wie sich die bei geplanten oder ungeplanten Ereignissen eingeleiteten automatischen Failovervorgänge ggf. auf die Anwendung auswirken, ehe Sie sie in der Produktionsumgebung einsetzen. Sie können eine spezielle API aufrufen, um eine Datenbank oder einen Pool für elastische Datenbanken neu zu starten, was wiederum ein Failover auslöst. Bei einer zonenredundanten Datenbank oder einem Pool für elastische Datenbanken führt der API-Aufruf dazu, dass Clientverbindungen von der Verfügbarkeitszone der alten primären Datenbank zur neuen primären Datenbank in einer anderen Verfügbarkeitszone umgeleitet werden. Zusätzlich zu den Tests, wie sich das Failover auf bestehende Datenbanksitzungen auswirkt, können Sie also auch prüfen, ob sich aufgrund von Änderungen an der Netzwerklatenz auch die Gesamtleistung ändert. Da Neustartvorgänge aufwendig sind und eine große Anzahl von ihnen die Plattform belasten könnte, ist für jede Datenbank oder jeden Pool für elastische Datenbanken nur alle 30 Minuten ein Failoveraufruf erlaubt. 

Ein Failover kann mithilfe der Rest-API oder mit PowerShell initiiert werden. Informationen zur Vorgehensweise mit der REST-API finden Sie unter [Datenbankfailover](https://docs.microsoft.com/rest/api/sql/databases(failover)/failover) und [Failover für einen Pool für elastische Datenbanken](https://docs.microsoft.com/rest/api/sql/elasticpools(failover)/failover). Informationen zur Vorgehensweise mit PowerShell finden Sie unter [Invoke-AzSqlDatabaseFailover](https://docs.microsoft.com/powershell/module/az.sql/invoke-azsqldatabasefailover) and [Invoke-AzSqlElasticPoolFailover](https://docs.microsoft.com/powershell/module/az.sql/invoke-azsqlelasticpoolfailover). Die Rest-API-Aufrufe können auch über die Azure CLI mithilfe des Befehls [az rest](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-rest) ausgeführt werden.

> [!IMPORTANT]
> Der Failoverbefehl ist derzeit in der Dienstebene „Hyperscale“ und für die verwaltete Instanz nicht verfügbar.

## <a name="conclusion"></a>Zusammenfassung

Azure SQL-Datenbank verfügt über eine integrierte Hochverfügbarkeitslösung, die tief in die Azure-Plattform integriert ist. Sie ist bei der Fehlererkennung und Wiederherstellung von Service Fabric, in Verbindung mit dem Datenschutz von Azure Blob Storage und für höhere Fehlertoleranz von Verfügbarkeitszonen abhängig. Darüber hinaus nutzt Azure SQL-Datenbank die Technologie der AlwaysOn-Verfügbarkeitsgruppen von SQL Server für Replikation und Failover. Dank der Kombination dieser Technologien können Anwendungen die Vorteile eines gemischten Speichermodells voll ausschöpfen und sehr anspruchsvolle SLAs unterstützen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Azure-Verfügbarkeitszonen](../availability-zones/az-overview.md)
- Weitere Informationen zu [Service Fabric](../service-fabric/service-fabric-overview.md)
- Weitere Informationen zu [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
- Weitere Optionen für Hochverfügbarkeit und Notfallwiederherstellung finden Sie unter [Geschäftskontinuität](sql-database-business-continuity.md).
