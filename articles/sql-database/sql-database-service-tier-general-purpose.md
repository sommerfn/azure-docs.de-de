---
title: Dienstebene „Universell“ – Azure SQL-Datenbank | Microsoft-Dokumentation
description: Informationen zur Dienstebene „Universell“ für Azure SQL-Datenbank
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 02/07/2019
ms.openlocfilehash: b4c0f88a1a21b0734c1652d19f74dc662d0a7ce1
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/04/2019
ms.locfileid: "73496209"
---
# <a name="general-purpose-service-tier---azure-sql-database"></a>Dienstebene „Universell“ – Azure SQL-Datenbank

> [!NOTE]
> Die Dienstebene „Universell“ beim vCore-basierten Kaufmodell wird beim DTU-basierten Kaufmodell als Dienstebene „Standard“ bezeichnet. Einen Vergleich zwischen vCore-basiertem Kaufmodell und DTU-basiertem Kaufmodell finden Sie unter [Kaufmodelle für Azure SQL-Datenbank und Ressourcen](sql-database-purchase-models.md).

Azure SQL-Datenbank basiert auf der an die Cloudumgebung angepasste Architektur der SQL Server-Datenbank-Engine, um selbst bei Infrastrukturausfällen eine Verfügbarkeit von 99,99 % sicherzustellen. In Azure SQL-Datenbank werden drei Dienstebenen verwendet, die jeweils unterschiedliche Architekturmodelle aufweisen. Diese Dienstebenen lauten:

- Allgemeiner Zweck
- Unternehmenskritisch
- Hyperscale

Das Architekturmodell für die Dienstebene „Universell“ basiert auf der Trennung von Compute- und Speicherebene. Dieses Architekturmodell beruht auf der Hochverfügbarkeit und Zuverlässigkeit von Azure Blob Storage mit transparenter Replikation von Datenbankdateien und garantiert ohne Datenverlust bei Ausfall der zugrunde liegenden Infrastruktur.

In der folgenden Abbildung sind vier Knoten im Architekturmodell des Typs „Standard“ mit getrennter Compute- und Speicherebene dargestellt.

![Trennung der Compute- und Speicherebene](media/sql-database-managed-instance/general-purpose-service-tier.png)

Das Architekturmodell für die Dienstebene „Universell“ umfasst zwei Ebenen:

- Eine zustandslose Computeebene, auf der der Prozess `sqlservr.exe` ausgeführt wird und die nur temporäre und zwischengespeicherte Daten (z.B. Plancache, Pufferpool, Spaltenspeicherpool) enthält. Dieser zustandslose SQL Server-Knoten wird von der Azure Service Fabric-Plattform gesteuert, die Prozesse initialisiert, die Integrität des Knotens steuert und bei Bedarf ein Failover zu einer anderen Stelle durchführt.
- Eine zustandsbehaftete Datenebene mit Datenbankdateien (MDF- und LDF-Dateien), die in Azure Blob Storage gespeichert sind. Azure Blob Storage garantiert die Vermeidung von Datenverlusten jeglicher Datensätze, die in Datenbankdateien platziert werden. Azure Storage verfügt über integrierte Datenverfügbarkeit und -redundanz, die sicherstellen, dass jeder Datensatz in einer Protokolldatei und jede Seite in einer Datendatei beibehalten wird, auch wenn der SQL Server-Prozess abstürzt.

Bei jeder Aktualisierung der Datenbank-Engine oder des Betriebssystems, bei Fehlern in Teilen der zugrunde liegenden Infrastruktur oder wenn im SQL Server-Prozess ein schwerwiegendes Problem erkannt wird, wird der zustandslose SQL Server-Prozess in Azure Service Fabric auf einen anderen zustandslosen Serverknoten verschoben. Es sind mehrere Reserveknoten vorhanden, auf denen im Fall eines Failovers des primären Knotens ein neuer Computedienst ausgeführt werden kann, um die Failoverzeit zu minimieren. Daten in der Azure Storage-Ebene sind nicht betroffen, und Daten- und Protokolldateien werden an den neu initialisierten SQL Server-Prozess angefügt. Dieser Prozess garantiert eine Verfügbarkeit von 99,99 %, kann jedoch aufgrund der Übergangszeit und der Tatsache, dass der neue SQL Server-Knoten mit „kaltem“ Cache gestartet wird, Auswirkungen auf die Leistung großer Workloads haben.

## <a name="when-to-choose-this-service-tier"></a>Wann sollte diese Dienstebene gewählt werden?

Die Dienstebene „Universell“ ist eine Standarddienstebene in Azure SQL-Datenbank, der für die meisten generischen Workloads entwickelt wurde. Wenn Sie eine vollständig verwaltete Datenbank-Engine mit einer SLA von 99,99 % mit einer Speicherlatenz von 5 bis 10 ms benötigen, die in den meisten Fällen der Azure SQL-IaaS-Lösung entspricht, ist der Tarif „Universell“ für Sie die richtige Wahl.

## <a name="next-steps"></a>Nächste Schritte

- Sehen Sie sich die Ressourcenmerkmale (Anzahl von Kernen, E/A, Arbeitsspeicher) der Ebene „Universell/Standard“ für eine [verwaltete Instanz](sql-database-managed-instance-resource-limits.md#service-tier-characteristics), für eine Einzeldatenbank im [V-Kern-Modell](sql-database-vcore-resource-limits-single-databases.md#general-purpose---provisioned-compute---gen4) bzw. [DTU-Modell](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes) oder für einen Pool für elastische Datenbanken im [V-Kern-Modell](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose---provisioned-compute---gen4) und [DTU-Modell](sql-database-dtu-resource-limits-elastic-pools.md#standard-elastic-pool-limits) an.
- Informationen zu den Tarifen [Unternehmenskritisch](sql-database-service-tier-business-critical.md) und [Hyperscale](sql-database-service-tier-hyperscale.md)
- Informationen zu [Service Fabric](../service-fabric/service-fabric-overview.md)
- Weitere Optionen zu Hochverfügbarkeit und Notfallwiederherstellung finden Sie unter [Geschäftskontinuität](sql-database-business-continuity.md).
