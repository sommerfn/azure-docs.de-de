---
title: Worum handelt es sich beim Azure SQL-Datenbank-Dienst?
description: Lernen Sie die technischen Details und Funktionen von Azure SQL-Datenbank kennen, des Managementsystems für relationale Datenbanken (RDBMS) von Microsoft in der Cloud.
keywords: Einführung in SQL, Was ist SQL-Datenbank?
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
ms.date: 04/08/2019
ms.openlocfilehash: 3932c22ff003a343e4c32aee117a7ddea922fbdb
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820741"
---
# <a name="what-is-the-azure-sql-database-service"></a>Worum handelt es sich beim Azure SQL-Datenbank-Dienst?

Azure SQL-Datenbank ist eine relationale Datenbank für allgemeine Zwecke, die als verwalteter Dienst bereitgestellt wird. Mithilfe dieses Diensts können Sie eine hochverfügbare und hochleistungsfähige Datenspeicherebene für Anwendungen und Lösungen in Azure erstellen. Azure SQL-Datenbank kann die richtige Wahl für eine Vielzahl moderner Cloudanwendungen sein, da die Lösung Ihnen ermöglicht, sowohl relationale Daten als auch [nicht relationale Strukturen](sql-database-multi-model-features.md) zu verarbeiten, wie z. B. Graph-, JSON-, räumliche und XML-Daten.

Sie basiert auf der neuesten stabilen Version der [Microsoft SQL Server-Datenbank-Engine](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json). Sie können erweiterte Funktionen zur Abfrageverarbeitung verwenden, wie z. B. [hochleistungsfähige In-Memory-Technologien](sql-database-in-memory.md) und [intelligente Abfrageverarbeitung](https://docs.microsoft.com/sql/relational-databases/performance/intelligent-query-processing?toc=/azure/sql-database/toc.json). Tatsächlich werden die neuesten SQL Server-Funktionen zunächst in Azure SQL-Datenbank und erst dann für SQL Server selbst veröffentlicht. Sie verfügen stets über die neuesten Features von SQL Server, die in Millionen Datenbanken getestet wurden, ohne den Mehraufwand für Patches oder Updates. 

SQL-Datenbank bietet zwei Kaufmodelle, mit denen sich die Leistung komfortabel definieren und skalieren lässt: ein [V-Kern-basiertes Kaufmodell](sql-database-service-tiers-vcore.md) und ein [DTU-basiertes Kaufmodell](sql-database-service-tiers-dtu.md). Bei Azure SQL-Datenbank handelt es sich um einen vollständig verwalteten Dienst mit eingebauter Hochverfügbarkeit, Sicherungs- und anderen gängigen Wartungsvorgängen. Microsoft kümmert sich um das Patchen und Aktualisieren des SQL- und Betriebssystemcodes. Sie müssen sich nicht um die zugrunde liegende Infrastruktur kümmern.

> [!NOTE]
> Relevante Begriffe und ihre Definitionen finden Sie im [Glossar mit Begriffen aus Azure SQL-Datenbank](sql-database-glossary-terms.md).

## <a name="deployment-models"></a>Bereitstellungsmodelle

Azure SQL-Datenbank bietet die folgenden Bereitstellungsoptionen für eine Azure SQL-Datenbank:

![Diagramm der Bereitstellungsoptionen](./media/sql-database-technical-overview/deployment-options.png)

- [Einzeldatenbank](sql-database-single-database.md) (auch Singleton genannt) stellt eine vollständig verwaltete, isolierte Datenbank dar. Sie können diese Option wählen, wenn Sie moderne Cloudanwendungen und Microservices einsetzen, die eine zentrale zuverlässige Datenquelle benötigen. Eine Einzeldatenbank ähnelt einer [eigenständigen Datenbank](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases?toc=/azure/sql-database/toc.json) in [Microsoft SQL Server-Datenbank-Engine](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json).
- Eine [verwaltete Instanz](sql-database-managed-instance.md) ist eine vollständig verwaltete Instanz der [Microsoft SQL Server-Datenbank-Engine](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation?toc=/azure/sql-database/toc.json). Sie enthält eine Gruppe von Datenbanken, die gemeinsam verwendet werden können. Verwenden Sie diese Option für die einfache Migration lokaler SQL Server-Datenbanken in die Azure-Cloud und für Anwendungen, die die Datenbankfunktionen der SQL Server-Datenbank-Engine nutzen müssen.
- Ein [Pool für elastische Datenbanken](sql-database-elastic-pool.md) ist eine Sammlung von [Einzeldatenbanken](sql-database-single-database.md) mit gemeinsam genutzten Ressourcen wie CPU oder Arbeitsspeicher. Singletons können in und aus Pools für elastische Datenbanken verschoben werden.

> [!IMPORTANT]
> Funktionelle Unterschiede zwischen Azure SQL-Datenbank und SQL Server sowie Unterschiede zwischen verschiedenen Bereitstellungsoptionen für Azure SQL-Datenbank finden Sie unter [Azure SQL-Datenbank – Funktionen](sql-database-features.md).

Azure SQL-Datenbank bietet eine vorhersagbare Leistung mit mehreren Ressourcentypen, Dienstebenen und Computegrößen. Die Lösung bietet dynamische Skalierbarkeit ohne Ausfallzeiten, integrierte intelligente Optimierung, globale Skalierbarkeit und Verfügbarkeit sowie erweiterte Sicherheitsoptionen. Mit diesen Funktionen können Sie sich auf die schnelle Entwicklung von Apps und die Verkürzung des Zeitraums bis zur Markteinführung konzentrieren, anstatt virtuelle Computer und Infrastruktur verwalten zu müssen. Der Azure SQL-Datenbank-Dienst wird derzeit in weltweit 38 Rechenzentren betrieben, sodass Sie Ihre Datenbank in einem Rechenzentrum in Ihrer Nähe ausführen können.

## <a name="scalable-performance-and-pools"></a>Skalierbare Leistung und Pools

Sie können die Menge der zugewiesenen Ressourcen festlegen. 
- Bei Einzeldatenbanken ist jede Datenbank von anderen isoliert und portierbar. Jede verfügt über einen eigenen garantierten Umfang an Compute-, Arbeitsspeicher- und Speicherressourcen. Der der Datenbank zugewiesene Ressourcenumfang ist allein der jeweiligen Datenbank vorbehalten und wird nicht gemeinsam mit anderen Datenbanken in Azure genutzt. Sie können [Ressourcen von Einzeldatenbanken dynamisch hoch- und herunterskalieren](sql-database-single-database-scale.md). Bei der Option „Einzeldatenbank“ werden verschiedene Compute-, Arbeitsspeicher- und Speicherressourcen für unterschiedliche Anforderungen bereitgestellt. Sie können beispielsweise 1 bis 80 virtuelle Kernen oder 32 GB bis 4 TB Speicherplatz erhalten. Mit der [Dienstebene „Hyperscale“](sql-database-service-tier-hyperscale.md) für Einzeldatenbanken können Sie bis zu 100 TB mit schnellen Sicherungs- und Wiederherstellungsfunktionen skalieren.
- Mit Pools für elastische Datenbanken können Sie Ressourcen zuweisen, die von allen Datenbanken im Pool gemeinsam genutzt werden. Sie können eine neue Datenbank erstellen oder bereits vorhandene Einzeldatenbanken in einen Ressourcenpool verschieben, um die Ressourcennutzung zu maximieren und Geld zu sparen. Diese Option bietet Ihnen auch die Möglichkeit zum dynamischen [Hoch- und Herunterskalieren von Ressourcen im Pool für elastische Datenbanken](sql-database-elastic-pool-scale.md).
- Bei verwalteten Instanzen ist jede verwaltete Instanz von den anderen Instanzen isoliert und verfügt über garantierte Ressourcen. Bei einer verwalteten Instanz verwenden die Instanzdatenbanken einen Satz von Ressourcen gemeinsam. Sie können [Ressourcen verwalteter Instanzen dynamisch hoch- und herunterskalieren](sql-database-managed-instance-resource-limits.md).

Sie können Ihre erste App für eine kleine Einzeldatenbank im Diensttarif „Universell“ zu niedrigen monatlichen Kosten erstellen. Sie können dann den Diensttarif jederzeit manuell oder programmgesteuert in „Unternehmenskritisch“ ändern, um den Anforderungen Ihrer Lösung gerecht zu werden. Die Leistungsanpassung ist möglich, ohne dass es für die App oder für Ihre Kunden zu Ausfallzeiten kommt. Durch die dynamische Skalierung kann Ihre Datenbank transparent auf schnell wechselnde Ressourcenanforderungen reagieren. Sie zahlen nur für die Ressourcen, die Sie im jeweiligen Augenblick benötigen.

*Dynamische Skalierbarkeit* ist nicht dasselbe wie *automatische Skalierung*. Bei der automatischen Skalierung wird ein Dienst automatisch auf der Grundlage von Kriterien skaliert. Die dynamische Skalierbarkeit ermöglicht dagegen eine manuelle Skalierung ohne Ausfallzeiten. Die Option „Einzeldatenbank“ unterstützt manuelle dynamische Skalierbarkeit, aber keine automatische Skalierung. Ein höheres Maß an Automatisierung lässt sich bei Bedarf mithilfe von Pools für elastische Datenbanken erzielen, die die gemeinsame Nutzung eines Ressourcenpools auf Grundlage individueller Datenbankanforderungen ermöglichen. Eine weitere Option sind Skripts, die Sie bei der Automatisierung der Skalierbarkeit einer Einzeldatenbank unterstützen. Ein Beispiel finden Sie unter [Überwachen und Skalieren einer Einzeldatenbank mit PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).

### <a name="purchasing-models"></a>Kaufmodelle

Für Azure SQL-Datenbank gibt es die folgenden Kaufmodelle:
- Beim [vCore-basierten Kaufmodell](sql-database-service-tiers-vcore.md) können Sie die Anzahl virtueller Kerne, die Arbeitsspeichermenge sowie Menge und Geschwindigkeit des Speichers auswählen. Mit dem vCore-basierten Kaufmodell können Sie auch den [Azure-Hybridvorteil für SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) nutzen, um Kosten einzusparen. Weitere Informationen zum Azure-Hybridvorteil finden Sie weiter unten in diesem Artikel unter „Häufig gestellten Fragen“.
- Das [DTU-basierte Kaufmodell](sql-database-service-tiers-dtu.md) bietet zur Unterstützung einfacher bis komplexer Datenbankworkloads eine Mischung aus Compute-, Arbeitsspeicher- und E/A-Ressourcen auf drei Dienstebenen. Die Computegrößen der einzelnen Ebenen bieten unterschiedliche Ressourcenzusammenstellungen, denen Sie zusätzliche Speicherressourcen hinzufügen können.
- Das [serverlose Modell](sql-database-serverless.md), welches die Computekapazität basierend auf dem Workloadbedarf skaliert und verwendete Computeressourcen nach Menge pro Sekunde abrechnet. Im Tarif „Serverloses Computing“ werden Datenbanken auch während inaktiver Zeiten, in denen nur Speicher in Rechnung gestellt wird, automatisch angehalten und bei Fortsetzen der Aktivität automatisch wieder in Betrieb genommen.

### <a name="service-tiers"></a>Dienstebenen

Azure SQL-Datenbank bietet drei Dienstebenen, die für unterschiedliche Anwendungstypen entwickelt wurden:
- Dienstebene [Universell/Standard](sql-database-service-tier-general-purpose.md), die für gemeinsame Workloads konzipiert ist. Diese Ebene bietet budgetorientierte ausgewogene Compute- und Speicheroptionen.
- Dienstebene [Unternehmenskritisch/Premium](sql-database-service-tier-business-critical.md) die für OLTP-Anwendungen mit hoher Transaktionsrate und den geringsten Latenzen bei E/A-Vorgängen konzipiert ist. Diese Ebene bietet dank mehrerer isolierter Replikate die höchste Resilienz gegenüber Ausfällen.
- Dienstebene [Hyperscale](sql-database-service-tier-hyperscale.md), die für sehr große OLTP-Datenbanken und die Möglichkeit zur automatischen Skalierung von Speicher sowie zur nahtlosen Skalierung von Computeressourcen konzipiert ist. 

### <a name="elastic-pools-to-maximize-resource-utilization"></a>Pools für elastische Datenbanken zum Maximieren der Ressourcenverwendung

Für viele Unternehmen und Anwendungen genügt es, wenn Einzeldatenbanken erstellt werden können und sich die Leistung nach oben oder unten anpassen lässt – insbesondere, wenn die Nutzungsmuster relativ gut vorhersagbar sind. Bei unvorhersagbaren Nutzungsmustern kann es schwer sein, die Kosten und Ihr Geschäftsmodell zu verwalten. [Pools für elastische Datenbanken](sql-database-elastic-pool.md) sind darauf ausgelegt, dieses Problem zu beheben. Sie weisen Leistungsressourcen einem Pool statt einer einzelnen Datenbank zu. Sie zahlen für die gemeinsam genutzten Leistungsressourcen des Pools und nicht für die Leistung einer Einzeldatenbank.

   ![Grafik, die Pools für elastische Datenbanken in den Editionen Basic, Standard und Premium zeigt](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Bei Pools für elastische Datenbanken müssen Sie sich nicht darauf konzentrieren, die Datenbankleistung nach oben oder unten anzupassen, wenn der Ressourcenbedarf schwankt. Die Pooldatenbanken nutzen die Leistungsressourcen des Pools für elastische Datenbanken je nach Bedarf. Pooldatenbanken nutzen die Grenzwerte des Pools, überschreiten sie aber nicht. Ihre Kosten bleiben also vorhersagbar, auch wenn sich die Nutzung der einzelnen Datenbanken nicht prognostizieren lässt.

Sie können [Datenbanken im Pool hinzufügen und entfernen](sql-database-elastic-pool-manage-portal.md)und so Ihre App von einigen wenigen auf Tausende Datenbanken skalieren, und zwar in einem kontrollierten Kostenrahmen. Sie können außerdem die minimalen und maximalen Ressourcen steuern, die für die Datenbanken im Pool verfügbar sind. So können Sie sicherstellen, dass keine Datenbank im Pool alle Poolressourcen verbraucht und dass jede Pooldatenbank über ein garantiertes Minimum an Ressourcen verfügt. Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen (Software-as-a-Service), für die Pools für elastische Datenbanken verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Skripts können Sie bei der Überwachung und Skalierung von Pools für elastische Datenbanken unterstützen. Ein Beispiel finden Sie unter [Verwenden von PowerShell zum Überwachen und Skalieren eines Pools für elastische Datenbanken in Azure SQL-Datenbank](scripts/sql-database-monitor-and-scale-pool-powershell.md).

> [!IMPORTANT]
> Eine verwaltete Instanz unterstützt keine Pools für elastische Datenbanken. Eine verwaltete Instanz ist vielmehr eine Sammlung von Instanzdatenbanken, die die Ressourcen der verwalteten Instanz gemeinsam nutzen.

### <a name="blend-single-databases-with-pooled-databases"></a>Mischen von Einzeldatenbanken mit Pooldatenbanken

Sie haben die Möglichkeit, Einzeldatenbanken mit Pools für elastische Datenbanken zu kombinieren und die Dienstebenen von Einzeldatenbanken und Pools für elastische Datenbanken an Ihre Bedürfnisse anzupassen. Sie können auch andere Azure-Dienste mit Azure SQL-Datenbank kombinieren, um Ihre individuellen Anforderungen bei der App-Entwicklung zu erfüllen, Kosten- und Ressourceneffizienz zu fördern und neue Geschäftsmöglichkeiten zu erschließen.

## <a name="extensive-monitoring-and-alerting-capabilities"></a>Umfassende Funktionen für Überwachung und Warnung

Azure SQL-Datenbank bietet erweiterte Funktionen und Tools für die Überwachung und Problembehandlung, mit deren Hilfe Sie sich detailliert über Merkmale von Workloads informieren können. Dazu zählen die Folgenden:
 - Die integrierten Überwachungsfunktionen, die von der neuesten Version der SQL Server-Datenbank-Engine bereitgestellt werden. Sie ermöglichen Ihnen Einblicke in die Leistung in Echtzeit. 
 - PaaS-Überwachungsfunktionen von Azure, mit denen Sie eine große Anzahl von Datenbankinstanzen überwachen und Probleme behandeln können.

[Abfragespeicher](sql-database-operate-query-store.md), eine integrierte SQL Server-Überwachungsfunktion, zeichnet die Leistung Ihrer Abfragen in Echtzeit auf und ermöglicht Ihnen, die potenziellen Leistungsprobleme und die wichtigsten Ressourcenverbraucher zu identifizieren. Über die automatische Optimierung und Empfehlungen werden Tipps für Abfragen mit Leistungsproblemen sowie für fehlende oder duplizierte Indizes bereitgestellt. Die automatische Optimierung in Azure SQL-Datenbank ermöglicht Ihnen, entweder manuell die Skripts auszuführen, die die Probleme beheben können, oder die Behebung von Azure SQL-Datenbank durchführen zu lassen. Azure SQL-Datenbank kann auch testen und bestätigen, dass die Korrektur einen gewissen Nutzen bietet, und die Änderung je nach Ergebnis beibehalten oder rückgängig machen. Neben dem Abfragespeicher und den automatischen Optimierungsfunktionen können Sie standardmäßige [DMVs und XEvent](sql-database-monitoring-with-dmvs.md) verwenden, um die Workloadleistung zu überwachen.

Mithilfe der integrierten Tools für [Leistungsüberwachung](sql-database-performance.md) und [Warnungen](sql-database-insights-alerts-portal.md) von Azure können Sie in Kombination mit den Leistungsbewertungen den Status mehrerer tausend Datenbanken überwachen. Mit diesen Tools können Sie die Auswirkungen des zentralen Hoch- oder Herunterskalierens je nach Ihren aktuellen oder projektbezogenen Leistungsanforderungen schnell bewerten. Darüber hinaus kann SQL-Datenbank zur einfacheren Überwachung [Metriken und Diagnoseprotokolle ausgeben](sql-database-metrics-diag-logging.md). Sie können SQL-Datenbank zum Speichern von Ressourcenverbrauch, Workern und Sitzungen sowie Verbindungen in einer der folgenden Azure-Ressourcen konfigurieren:

- **Azure Storage**: Ermöglicht die kostengünstige Archivierung großer Mengen von Telemetriedaten
- **Azure Event Hubs**: Ermöglicht die Integration von Telemetriedaten von SQL-Datenbank in Ihre benutzerdefinierte Überwachungslösung oder in Hotpipelines
- **Azure Monitor-Protokolle:** Ermöglicht die Verwendung einer integrierten Überwachungslösung mit Funktionen für Berichterstellung, Warnungen und Problemlösung.

![Diagramm der Azure-Überwachungsarchitektur](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="availability-capabilities"></a>Verfügbarkeitsfunktionen

In einer herkömmlichen SQL Server-Umgebung sind im Allgemeinen mindestens zwei Computer lokal eingerichtet. Diese Computer verfügen über synchron gepflegte exakte Kopien der Daten, um Schutz vor einem Ausfall eines einzelnen Geräts oder einer einzelnen Komponente zu bieten. Diese Umgebung gewährleistet Hochverfügbarkeit, bietet aber keinen Schutz vor einer Naturkatastrophe, die Ihr Rechenzentrum zerstört.

Die Notfallwiederherstellung geht davon aus, dass ein katastrophales Ereignis geografisch lokal so begrenzt ist, dass es an entfernter Stelle einen anderen Computer bzw. eine andere Gruppe von Computern mit einer Kopie Ihrer Daten gibt. In SQL Server können Sie AlwaysOn-Verfügbarkeitsgruppen im asynchronen Modus verwenden, um diese Vorgabe zu erfüllen. Benutzer möchten oft nicht warten, bis die Replikation so weit entfernt erfolgt ist, bevor sie eine Transaktion committen, sodass ein Potenzial für Datenverluste besteht, wenn Sie ungeplante Failover durchführen.

Datenbanken auf Premium- und unternehmenskritischen Dienstebenen [leisten bereits etwas sehr Ähnliches](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) für die Synchronisierung einer Verfügbarkeitsgruppe. Datenbanken auf niedrigeren Dienstebenen bieten Redundanz mithilfe von Speicher, der einen [anderen, aber vergleichbaren Mechanismus](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) verwendet. Es gibt integrierte Logik, die Schutz vor dem Ausfall eines einzelnen Computers bietet. Das Feature „Aktive Georeplikation“ bietet Ihnen die Möglichkeit, sich vor Katastrophen zu schützen, bei denen eine ganze Region zerstört wird.

Der Dienst Azure-Verfügbarkeitszonen versucht, innerhalb einer Region Schutz vor Ausfall eines einzelnen Rechenzentrumsgebäudes zu bieten. Dies bedeutet für ein Gebäude Schutz vor dem Verlust der Stromversorgung oder des Netzwerks. In Azure SQL-Datenbank platzieren Sie die verschiedenen Replikate in verschiedenen Verfügbarkeitszonen (praktisch in verschiedenen Gebäuden).

Tatsächlich trägt die Vereinbarung zum Servicelevel [(SLA)](https://azure.microsoft.com/support/legal/sla/) von Azure, die von einem globalen Netzwerk von Microsoft verwalteter Rechenzentren unterstützt wird, dazu bei, dass Ihre App rund um die Uhr läuft. Die Azure-Plattform übernimmt die vollständige Verwaltung jeder Datenbank und garantiert die Vermeidung von Datenverlust und einen sehr hohen Grad an Datenverfügbarkeit. In Azure werden Bereiche wie Patchen, Sicherungen, Replikation, Fehlererkennung, zugrunde liegende potenzielle Hardware-, Software- oder Netzwerkfehler, Bereitstellung von Fehlerbehebungen, Failover, Datenbankupgrades und andere Wartungsaufgaben automatisch durchgeführt. Die Standardverfügbarkeit wird erreicht, indem eine Unterteilung in Compute- und Speicherebenen vorgenommen wird. Premium-Verfügbarkeit wird erreicht, indem Compute- und Speicherbereich zur Steigerung der Leistung auf einem einzelnen Knoten angeordnet und dann Technologien implementiert werden, die mit Always On-Verfügbarkeitsgruppen vergleichbar sind. Eine vollständige Beschreibung der Funktionen für Hochverfügbarkeit von Azure SQL-Datenbank finden Sie unter [Verfügbarkeit von Azure SQL-Datenbank](sql-database-high-availability.md). 

Azure SQL-Datenbank bietet außerdem integrierte Features für [Geschäftskontinuität und globale Skalierbarkeit](sql-database-business-continuity.md). Das umfasst:

- [Automatische Sicherungen](sql-database-automated-backups.md):

  Azure SQL-Datenbank führt automatisch vollständige, differenzielle und Transaktionsprotokollsicherungen von Azure SQL-Datenbank-Instanzen durch, sodass Sie diese zu einem beliebigen Zeitpunkt wiederherstellen können. Für Einzel- und Pooldatenbanken können Sie Azure SQL-Datenbank zum Speichern von vollständigen Datenbanksicherungen in Azure Storage für die langfristige Aufbewahrung konfigurieren. Für verwaltete Instanzen können Sie auch ausschließlich kopierbare Sicherungen für die langfristige Sicherungsaufbewahrung ausführen.

- [Zeitpunktwiederherstellungen](sql-database-recovery-using-backups.md):

  Alle Bereitstellungsoptionen von Azure SQL-Datenbank unterstützen die zeitpunktbezogene Wiederherstellung innerhalb des Aufbewahrungszeitraums für automatische Sicherungen sämtlicher Azure SQL-Datenbank-Instanzen.
- [Aktive Georeplikation](sql-database-active-geo-replication.md):

  Bei Einzel- und Pooldatenbanken können Sie bis zu vier lesbare sekundäre Datenbanken konfigurieren – entweder im gleichen Azure-Rechenzentrum oder in weltweit verteilten Azure-Rechenzentren. Wenn Sie beispielsweise über eine SaaS-Anwendung mit einer Katalogdatenbank verfügen, die eine große Anzahl gleichzeitiger schreibgeschützter Transaktionen aufweist, können Sie mithilfe der aktiven Georeplikation eine globale Leseskalierung aktivieren. So können Sie Engpässe in der primären Datenbank beseitigen, die durch hohe Workloads aufgrund von Lesevorgängen verursacht werden. Verwenden Sie für verwaltete Instanzen Autofailover-Gruppen.
- [Autofailover-Gruppen](sql-database-auto-failover-group.md):

  Alle Bereitstellungsoptionen von Azure SQL-Datenbank ermöglichen den Einsatz von Failovergruppen, um Hochverfügbarkeit und Lastenausgleich in globalem Maßstab zu ermöglichen. Dazu gehören die transparente Georeplikation und das Failover großer Mengen von Datenbanken, Pools für elastische Datenbanken und verwalteten Instanzen. Failovergruppen ermöglichen bei minimalem Verwaltungsaufwand die Erstellung global verteilter SaaS-Anwendungen. Dadurch bleiben alle komplexen Überwachungs-, Routing- und Failoverorchestrierungen für Azure SQL-Datenbank erhalten.
- [Zonenredundante Datenbanken](sql-database-high-availability.md):

  SQL-Datenbank ermöglicht die Bereitstellung von Premium-Datenbanken, unternehmenskritischen Datenbanken oder Pools für elastische Datenbanken über mehrere Verfügbarkeitszonen hinweg. Datenbanken und Pools für elastische Datenbanken verfügen über mehrere redundante Replikate, um Hochverfügbarkeit sicherzustellen. Die Anordnung dieser Replikate in mehreren Verfügbarkeitszonen sorgt für mehr Resilienz. Dies schließt die automatische Wiederherstellung nach Rechenzentrumsausfällen ohne Datenverlust ein.

## <a name="built-in-intelligence"></a>Integrierte Logik

Azure SQL-Datenbank stellt integrierte Logik bereit, die Ihnen hilft, den Aufwand für die Ausführung und Verwaltung von Datenbanken erheblich zu senken und Leistung und Sicherheit Ihrer Anwendung zu maximieren. Bei der Ausführung von Millionen von Kundenworkloads rund um die Uhr sammelt und verarbeitet Azure SQL-Datenbank eine riesige Menge an Telemetriedaten. Dabei bleibt die Privatsphäre des Kunden jedoch vollständig gewahrt. Verschiedene Algorithmen werten fortlaufend die Telemetriedaten aus, damit der Dienst mit Ihrer Anwendung lernen und sich anpassen kann.

### <a name="automatic-performance-monitoring-and-tuning"></a>Automatische Leistungsüberwachung und -optimierung

SQL-Datenbank bietet detaillierte Einblicke in die Abfragen, die Sie überwachen sollten. Azure SQL-Datenbank erlernt Ihre Datenbankmuster und ermöglicht Ihnen das Anpassen Ihres Datenbankschemas an Ihre Workload. SQL-Datenbank stellt [Empfehlungen zur Leistungsoptimierung](sql-database-advisor.md) bereit, mit denen Sie Optimierungsschritte überprüfen und anwenden können.

Eine kontinuierliche Überwachung der Datenbank ist jedoch eine schwierige und aufwendige Aufgabe, insbesondere bei sehr vielen Datenbanken. [Intelligent Insights](sql-database-intelligent-insights.md) nimmt Ihnen diese Aufgabe ab und überwacht automatisch nach Maß die Leistung von Azure SQL-Datenbank. Das Tool informiert Sie über Leistungseinbußen, identifiziert die Ursache jedes Problems und gibt nach Möglichkeit Empfehlungen zur Leistungssteigerung.

Das Verwalten einer großen Anzahl von Datenbanken ist eventuell nicht möglich, selbst mit allen verfügbaren Tools und Berichten, die Azure SQL-Datenbank und Azure bereitstellen. Anstelle der manuellen Überwachung und Optimierung der Datenbank sollten Sie erwägen, einige der Überwachungs- und Optimierungsaktionen mithilfe der [automatischen Optimierung](sql-database-automatic-tuning.md) an Azure SQL-Datenbank zu delegieren. SQL-Datenbank wendet Empfehlungen automatisch an und testet und überprüft jede der Optimierungsaktionen, um sicherzustellen, dass sich die Leistung weiter verbessert. Auf diese Weise passt sich SQL-Datenbank automatisch auf kontrollierte und sichere Weise an Ihre Workload an. Die automatische Optimierung bedeutet, dass die Leistung Ihrer Datenbank vor und nach jeder Optimierungsaktion sorgfältig überwacht und verglichen wird. Wenn sich die Leistung nicht verbessert, wird die Optimierungsaktion rückgängig gemacht.

Viele unserer Partner, die [mehrinstanzenfähige SaaS-Apps](sql-database-design-patterns-multi-tenancy-saas-applications.md) mithilfe von Azure SQL-Datenbank ausführen, verlassen sich auf die automatische Leistungsoptimierung, um sicherzustellen, dass ihre Anwendungen stets eine stabile und zuverlässige Leistung zeigen. Für sie verringert dieses Feature das Risiko, dass mitten in der Nacht ein Leistungsproblem auftritt, enorm. Da darüber hinaus auch ein Teil ihres Kundenstamms SQL Server einsetzt, setzen sie die Indizierungsempfehlungen von Azure SQL-Datenbank um, um ihre SQL Server-Kunden zu unterstützen.

Zwei Aspekte der automatischen Optimierung sind [in Azure SQL-Datenbank verfügbar](sql-database-automatic-tuning.md):

- **Automatische Indexverwaltung**: Diese Funktion identifiziert Indizes, die der Datenbank hinzugefügt, und solche, die entfernt werden sollten.
- **Automatische Plankorrektur**: Diese Funktion erkennt problematische Pläne und korrigiert Leistungsprobleme mit SQL-Plänen.

### <a name="adaptive-query-processing"></a>Adaptive Abfrageverarbeitung

Sie können die [adaptive Abfrageverarbeitung](/sql/relational-databases/performance/intelligent-query-processing) nutzen. Diese schließt auch die überlappende Ausführung für Tabellenwertfunktionen mit mehreren Anweisungen, Feedback zur Speicherzuweisung im Batchmodus und adaptive Joins im Batchmodus ein. Jedes dieser Features zur adaptiven Abfrageverarbeitung wendet ähnliche Techniken zum Lernen und Anpassen an, um weitere Leistungsprobleme zu beheben, die durch traditionell schwierig zu lösende Probleme bei der Abfrageoptimierung verursacht werden.

## <a name="advanced-security-and-compliance"></a>Erweiterte Sicherheit und Konformität

SQL-Datenbank bietet eine Reihe von [integrierten Sicherheits- und Konformitätsfeatures](sql-database-security-overview.md), mit der Sie Ihre Anwendung an verschiedene Sicherheits- und Konformitätsanforderungen anpassen können.

> [!IMPORTANT]
> Microsoft hat Azure SQL-Datenbank (alle Bereitstellungsoptionen) für eine Reihe von Konformitätsstandards zertifizieren lassen. Weitere Informationen finden Sie im [Microsoft Azure Trust Center](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942), wo die aktuelle Liste von Konformitätszertifizierungen für Azure SQL-Datenbank angezeigt wird.

### <a name="advance-threat-protection"></a>Advance Threat Protection

Advanced Data Security ist ein einheitliches Paket für erweiterte SQL-Sicherheitsfunktionen. Dazu zählen die Funktion zur Ermittlung und Klassifizierung sensibler Daten, die Verwaltung von Datenbankrisiken und die Erkennung ungewöhnlicher Aktivitäten, die eine Bedrohung für Ihre Datenbank darstellen können. Hier können Sie diese Funktionen an einer Stelle aktivieren und verwalten.

- [Datenermittlung und -klassifizierung](sql-database-data-discovery-and-classification.md):

  Dieses Feature bietet in Azure SQL-Datenbank integrierte Funktionen zum Ermitteln, Klassifizieren, Bezeichnen und Schützen sensibler Daten in Ihren Datenbanken. Es kann Einblicke in den Zustand Ihrer Datenbankklassifizierung bereitstellen und den Zugriff auf sensible Daten innerhalb der Datenbank und außerhalb ihrer Grenzen verfolgen.
- [Sicherheitsrisikobewertung:](sql-vulnerability-assessment.md)

  Dieser Dienst kann potenzielle Schwachstellen in der Datenbank ermitteln, nachverfolgen und Sie bei der Beseitigung unterstützen. Er bietet Einblicke in Ihren Sicherheitsstatus, enthält umsetzbare Schritte zum Beheben von Sicherheitsproblemen und verbessert Ihre Datenbanksicherheit.
- [Bedrohungserkennung:](sql-database-threat-detection.md)

  Dieses Feature erkennt anormale Aktivitäten, die auf ungewöhnliche und potenziell schädliche Versuche hindeuten, auf Ihre Datenbank zuzugreifen oder sie missbräuchlich zu nutzen. Sie überwacht Ihre Datenbank fortlaufend auf verdächtige Aktivitäten und bietet sofortige Sicherheitswarnungen zu potenziellen Sicherheitslücken, Angriffe durch Einschleusung von SQL-Befehlen und ungewöhnliche Datenbankzugriffsmuster. Die Warnungen der Bedrohungserkennung enthalten Details zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann.

### <a name="auditing-for-compliance-and-security"></a>Überwachung auf Konformität und Sicherheit

Die [Überwachung](sql-database-auditing.md) verfolgt Datenbankereignisse und schreibt diese in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto. Die Überwachung kann Sie dabei unterstützen, gesetzliche Bestimmungen einzuhalten, Datenbankaktivitäten nachzuvollziehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftliche Probleme oder mutmaßliche Sicherheitsverstöße hinweisen können.

### <a name="data-encryption"></a>Datenverschlüsselung

Azure SQL-Datenbank ermöglicht den Schutz Ihrer Daten mithilfe der folgenden Verschlüsselungsmöglichkeiten. Verschlüsseln von Daten während der Übertragung durch [Transport Layer Security (TLS)](https://support.microsoft.com/kb/3135244). Verschlüsseln von ruhenden Daten mittels [transparenter Datenverschlüsselung](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Verschlüsseln Daten bei ihrer Nutzung durch [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine).

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory-Integration und Multi-Factor Authentication

SQL-Datenbank ermöglicht über die [Azure Active Directory-Integration](sql-database-aad-authentication.md) eine zentrale Verwaltung von Identitäten der Datenbankbenutzer und anderer Microsoft-Dienste. Diese Funktion vereinfacht die Berechtigungsverwaltung und erhöht die Sicherheit. Azure Active Directory unterstützt die [mehrstufige Authentifizierung](sql-database-ssms-mfa-authentication.md), um die Daten- und Anwendungssicherheit zu erhöhen, während gleichzeitig einmaliges Anmelden unterstützt wird.

## <a name="easy-to-use-tools"></a>Benutzerfreundliche Tools

SQL-Datenbank sorgt für einfachere und produktivere Abläufe beim Erstellen und Verwalten von Anwendungen. Mit SQL-Datenbank können Sie sich auf das konzentrieren, was Sie am besten können: erstklassige Apps entwickeln. Sie können in Azure SQL-Datenbank Verwaltungs- und Entwicklungsaufgaben mithilfe von Tools und Fertigkeiten erledigen, über die Sie bereits verfügen.

- [Das Azure-Portal](https://portal.azure.com/):

  Eine webbasierte Anwendung für die Verwaltung aller Azure-Dienste.
- [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms):

  Eine kostenlose, herunterladbare Clientanwendung für die Verwaltung beliebiger SQL-Infrastrukturen – von SQL Server bis Azure SQL-Datenbank.
- [SQL Server Data Tools in Visual Studio](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt):

  Eine kostenlose, herunterladbare Clientanwendung, mit der Sie relationale SQL Server-Datenbanken, Azure SQL-Datenbank-Instanzen, Integration Services-Pakete, Analysis Services-Datenmodelle und Reporting Services-Berichte erstellen können.
- [Visual Studio Code](https://code.visualstudio.com/docs):

  Kostenloser, herunterladbarer Open-Source-Code-Editor für Windows, macOS und Linux. Dieser Editor unterstützt Erweiterungen, etwa die [Erweiterung MSSQLSERVER](https://aka.ms/mssql-marketplace), zum Abfragen von Microsoft SQL Server, Azure SQL-Datenbank und Azure SQL Data Warehouse.

Azure SQL-Datenbank unterstützt unter macOS, Linux und Windows das Erstellen von Anwendungen mit Python, Java, Node.js, PHP, Ruby und .NET. SQL-Datenbank unterstützt dieselben [Verbindungsbibliotheken](sql-database-libraries.md) wie SQL Server.

[!INCLUDE [sql-database-create-manage-portal](includes/sql-database-create-manage-portal.md)]

## <a name="sql-database-frequently-asked-questions"></a>Häufig gestellte Fragen zu Azure SQL-Datenbank

### <a name="what-is-the-current-version-of-sql-database"></a>Wie lautet die aktuelle Version von SQL-Datenbank?

Die aktuelle Version von SQL-Datenbank ist V12. Version V11 wurde eingestellt.

### <a name="can-i-control-when-patching-downtime-occurs"></a>Kann ich den Zeitpunkt von Ausfallzeiten beim Patchen steuern?

Nein. Die Auswirkungen vom Patchen sind im Allgemeinen nicht merklich, wenn Sie in Ihrer App [Wiederholungslogik einsetzen](sql-database-develop-overview.md#resiliency). Weitere Informationen finden Sie [Planen von Azure-Wartungsereignissen in Azure SQL-Datenbank](sql-database-planned-maintenance.md).

### <a name="azure-hybrid-benefit-questions"></a>Fragen zum Azure-Hybridvorteil

#### <a name="are-there-dual-use-rights-with-azure-hybrid-benefit-for-sql-server"></a>Gibt es für den Azure-Hybridvorteil für SQL Server Dualnutzungsrechte?

Sie verfügen für die Lizenz 180 Tage lang über Dualnutzungsrechte, um sicherzustellen, dass die Migrationen reibungslos durchgeführt werden können. Nach diesem Zeitraum von 180 Tagen können Sie die SQL Server-Lizenz nur in der Cloud in Azure SQL-Datenbank verwenden. Sie verfügen nicht mehr über duale Nutzungsrechte für die lokale und die Cloudumgebung.

#### <a name="how-does-azure-hybrid-benefit-for-sql-server-differ-from-license-mobility"></a>Wie unterscheidet sich der Azure-Hybridvorteil für SQL Server von der License Mobility?

Wir bieten SQL Server-Kunden License Mobility-Leistungen im Rahmen von Software Assurance. Dies ermöglicht die erneute Zuweisung Ihrer Lizenzen zu mit einem Partner gemeinsam genutzten Servern. Sie können diese Leistung für Azure IaaS und AWS EC2 in Anspruch nehmen.

Der Azure-Hybridvorteil für SQL Server unterscheidet sich in zwei wichtigen Bereichen von License Mobility:

- Er ist mit wirtschaftlichen Vorteilen in Bezug auf das Verlagern von hochgradig virtualisierten Workloads nach Azure verbunden. Kunden mit SQL Server Enterprise Edition können in Azure für die SKU „Universell“ für jeden Kern, der lokal vorhanden ist, vier Kerne für hochgradig virtualisierte Anwendungen erhalten. Bei License Mobility ergeben sich bei der Verlagerung virtualisierter Workloads in die Cloud keine besonderen Kostenvorteile.
- Hierbei ist ein PaaS-Ziel in Azure (eine verwaltete Azure SQL-Datenbank) vorhanden, das mit der lokalen Azure SQL Server-Instanz in hohem Maß kompatibel ist.

#### <a name="what-are-the-specific-rights-of-the-azure-hybrid-benefit-for-sql-server"></a>Welche spezifischen Rechte gelten für den Azure-Hybridvorteil für SQL Server?

Azure SQL-Datenbank-Kunden bietet Azure-Hybridvorteil für SQL Server die folgenden Rechte:

|Lizenzspeicherbedarf|Was erhalten Sie beim Azure-Hybridvorteil für SQL Server?|
|---|---|
|SQL Server Enterprise Edition – Hauptkunden mit SA|<li>Zahlung des Basistarifs für SKU „Universell“ oder „Unternehmenskritisch“</li><br><li>1 Kern lokal = 4 Kerne in SKU „Universell“</li><br><li>1 Kern lokal = 1 Kern in SKU „Unternehmenskritisch“</li>|
|SQL Server Standard Edition – Hauptkunden mit SA|<li>Zahlung des Basistarifs nur für SKU „Universell“</li><br><li>1 Kern lokal = 1 Kern in SKU „Universell“</li>|
|||

## <a name="engage-with-the-sql-server-engineering-team"></a>Kontakt mit dem SQL Server-Entwicklungsteam aufnehmen

- [DBA Stack Exchange](https://dba.stackexchange.com/questions/tagged/sql-server): Stellen Sie Fragen zur Datenbankverwaltung.
- [Stack Overflow](https://stackoverflow.com/questions/tagged/sql-server): Stellen Sie Fragen zur Entwicklung.
- [MSDN-Foren](https://social.msdn.microsoft.com/Forums/home?category=sqlserver): Stellen Sie technische Fragen.
- [Feedback](https://aka.ms/sqlfeedback): Hier können Sie Fehler melden und Features anfordern.
- [Reddit](https://www.reddit.com/r/SQLServer/): Tauschen Sie sich über SQL Server aus.

## <a name="next-steps"></a>Nächste Schritte

- Auf der Seite [Preise](https://azure.microsoft.com/pricing/details/sql-database/) finden Sie Preisvergleiche und Rechner für Einzeldatenbanken und Pools für elastische Datenbanken.
- Diese Schnellstarts erleichtern Ihnen den Einstieg:

  - [Create a SQL database in the Azure portal](sql-database-single-database-get-started.md) (Erstellen einer SQL-Datenbank im Azure-Portal)  
  - [Erstellen einer SQL-Datenbank mit der Azure CLI](sql-database-get-started-cli.md)
  - [Erstellen einer SQL-Datenbank mithilfe von PowerShell](sql-database-get-started-powershell.md)

- Azure CLI- und PowerShell-Beispiele finden Sie unter:
  - [Azure CLI-Beispiele für SQL-Datenbank](sql-database-cli-samples.md)
  - [Azure PowerShell-Beispiele für SQL-Datenbank](sql-database-powershell-samples.md)

- Informationen zu angekündigten neuen Funktionen finden Sie unter [Azure-Updates](https://azure.microsoft.com/roadmap/?category=databases).
- Siehe den [Blog zu Azure SQL-Datenbank](https://azure.microsoft.com/blog/topics/database), in dem Sie Beiträge von Mitgliedern des SQL Server-Produktteams zu Neuerungen und Features von Azure SQL-Datenbank finden.

