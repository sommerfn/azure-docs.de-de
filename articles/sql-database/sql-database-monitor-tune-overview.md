---
title: Überwachen und Optimieren der Datenbankleistung
description: Tipps für die Leistungsoptimierung in Azure SQL-Datenbank durch Auswertung und Verbesserung.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jrasnick, carlrab
ms.date: 01/25/2019
ms.openlocfilehash: e77af00dc3352af3265da90685e58b34c96bee81
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73825159"
---
# <a name="monitoring-and-performance-tuning"></a>Überwachen und Optimieren der Datenbankleistung

Azure SQL-Datenbank bietet Tools und Methoden, mit denen Sie problemlos die Nutzung überwachen, Ressourcen (CPU, Arbeitsspeicher, E/A) hinzufügen oder entfernen, potenzielle Probleme behandeln sowie Empfehlungen zur Verbesserung der Leistung einer Datenbank erhalten können. Azure SQL-Datenbank verfügt über Features zur automatischen Behebung von Problemen in den Datenbanken. 

Die automatische Optimierung ermöglicht einer Datenbank die Anpassung an die Workload und die automatische Optimierung der Leistung. Für einige spezielle Probleme muss jedoch unter Umständen eine Problembehandlung durchgeführt werden. In diesem Artikel werden einige Best Practices und Tools für die Behandlung von Leistungsproblemen beschrieben.

Um den reibungslosen Betrieb einer Datenbank zu gewährleisten, sollten Sie folgende Maßnahmen ergreifen:
- [Überwachen der Datenbankleistung](#monitor-database-performance), um sicherzustellen, dass die Ressourcen, die der Datenbank zugewiesen wurden, die Workload verarbeiten können. Wenn die Datenbank an ihre Ressourcengrenzen stößt, haben Sie folgende Möglichkeiten:
   - Ermitteln und Optimieren der Abfragen mit dem höchsten Ressourcenverbrauch
   - Hinzufügen weiterer Ressourcen durch ein [Upgrade der Dienstebene](https://docs.microsoft.com/azure/sql-database/sql-database-scale-resources)
- [Behandeln von Leistungsproblemen](#troubleshoot-performance-problems), um zu ermitteln, warum ein potenzielles Problem aufgetreten ist, und um die Grundursache des Problems zu bestimmen. Ergreifen Sie anschließend entsprechende Maßnahmen, um das Problem zu beheben.

## <a name="monitor-database-performance"></a>Überwachen der Datenbankleistung

Die Überwachung der Leistung einer SQL-Datenbank in Azure beginnt mit der Überwachung der Ressourcennutzung relativ zur gewählten Datenbankleistung. Überwachen Sie die folgenden Ressourcen:
 - **CPU-Auslastung:** Überprüfen Sie, ob die Datenbank über einen längeren Zeitraum eine CPU-Auslastung von 100 Prozent erreicht. Eine hohe CPU-Auslastung kann darauf hindeuten, dass Abfragen, die besonders viel Rechenleistung beanspruchen, identifiziert und optimiert werden müssen. Außerdem kann eine hohe CPU-Auslastung darauf hindeuten, dass für die Datenbank oder Instanz ein Upgrade auf eine höhere Dienstebene erforderlich ist. 
 - **Wartestatistik:** Ermitteln Sie mithilfe von [sys.dm_os_wait_stats (Transact-SQL)](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql) die Wartezeit von Abfragen. Abfragen können auf Ressourcen, Warteschlangen oder externe Vorgänge warten. 
 - **E/A-Nutzung:** Überprüfen Sie, ob die Datenbank die E/A-Grenzwerte des zugrunde liegenden Speichers erreicht.
 - **Arbeitsspeicherauslastung:** Die Menge des für die Datenbank oder Instanz verfügbaren Arbeitsspeichers ist proportional zur Anzahl virtueller Kerne. Vergewissern Sie sich, dass der Arbeitsspeicher für die Workload ausreichend ist. Die Seitenlebenserwartung ist einer der Parameter, die darauf hinweisen können, wie schnell die Seiten aus dem Arbeitsspeicher entfernt werden.

Der Azure SQL-Datenbankdienst enthält Tools und Ressourcen, die Sie bei der Behandlung und Behebung potenzieller Leistungsprobleme unterstützen. Unter [Empfehlungen zur Leistung für SQL-Datenbank](sql-database-advisor.md) erfahren Sie, wie Sie Möglichkeiten zur Verbesserung und Optimierung der Abfrageleistung ohne Ressourcenänderungen identifizieren. 

Fehlende Indizes und falsch optimierte Abfragen sind häufige Ursachen für eine schlechte Datenbankleistung. Sie können Optimierungsempfehlungen anwenden, um die Leistung der Workload zu verbessern. Sie können die [Leistung der Abfragen aber auch automatisch durch Azure SQL-Datenbank optimieren lassen](sql-database-automatic-tuning.md), indem Sie alle identifizierten Empfehlungen anwenden. Vergewissern Sie sich anschließend, dass sich die Datenbankleistung durch die Empfehlungen verbessert hat.

> [!NOTE]
> Die Indizierung ist nur in Einzeldatenbanken und in Pools für elastische Datenbanken verfügbar. In einer verwalteten Instanz steht die Indizierung nicht zur Verfügung.

Verwenden Sie eine der folgenden Optionen, um die Datenbankleistung zu überwachen und Probleme mit der Datenbankleistung zu behandeln:

- Wählen Sie im [Azure-Portal](https://portal.azure.com) die Option **SQL-Datenbanken** und anschließend die Datenbank aus. Suchen Sie im Diagramm **Überwachung** nach Ressourcen, die nahezu vollständig ausgelastet sind. „DTU-Verbrauch“ wird standardmäßig angezeigt. Wählen Sie **Bearbeiten** aus, um den Zeitraum und die angezeigten Werte zu ändern.
- Tools wie SQL Server Management Studio bieten zahlreiche praktische Berichte (beispielsweise das [Leistungsdashboard](https://docs.microsoft.com/sql/relational-databases/performance/performance-dashboard)). Verwenden Sie diese Berichte, um die Ressourcennutzung zu überwachen und Abfragen mit besonders hohem Ressourcenverbrauch zu ermitteln. Mithilfe des [Abfragespeichers](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store#Regressed) können Sie Abfragen mit rückläufiger Leistung ermitteln.
- Verwenden Sie [Query Performance Insight](https://portal.azure.com) im [Azure-Portal](sql-database-query-performance.md), um die Abfragen mit dem höchsten Ressourcenverbrauch zu ermitteln. Dieses Feature ist nur in Einzeldatenbanken und in Pools für elastische Datenbanken verfügbar.
- Verwenden Sie [SQL Database Advisor](sql-database-advisor-portal.md), um Empfehlungen zum Erstellen und Löschen von Indizes, zum Parametrisieren von Abfragen und zum Beheben von Schemaproblemen anzuzeigen. Dieses Feature ist nur in Einzeldatenbanken und in Pools für elastische Datenbanken verfügbar.
- Verwenden Sie [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md) zur automatischen Überwachung der Datenbankleistung. Wenn ein Leistungsproblem erkannt wird, wird ein Diagnoseprotokoll generiert. Das Protokoll enthält Details und eine Ursachenanalyse (Root Cause Analysis, RCA) für das Problem. Nach Möglichkeit wird auch eine Empfehlung zur Verbesserung der Leistung bereitgestellt.
- [Aktivieren Sie die automatische Optimierung](sql-database-automatic-tuning-enable.md), um Leistungsprobleme automatisch durch Azure SQL-Datenbank beheben zu lassen.
- Verwenden Sie [dynamische Verwaltungssichten (Dynamic Management Views, DMVs)](sql-database-monitoring-with-dmvs.md), [erweiterte Ereignisse](sql-database-xevent-db-diff-from-svr.md) und den [Abfragespeicher](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store), um ausführlichere Informationen zur Behandlung von Leistungsproblemen zu erhalten.

> [!TIP]
> Sehen Sie sich nach der Identifizierung eines Leistungsproblems unseren [Leistungsleitfaden](sql-database-performance-guidance.md) an, um Techniken zur Verbesserung der Leistung von Azure SQL-Datenbank zu finden.

## <a name="troubleshoot-performance-problems"></a>Behandeln von Leistungsproblemen

Beim Diagnostizieren und Beheben von Leistungsproblemen besteht der erste Schritt darin, den Zustand der einzelnen aktiven Abfragen sowie die Bedingungen zu ermitteln, die zu Leistungsproblemen in Bezug auf den jeweiligen Workloadzustand führen. Um die Leistung von Azure SQL-Datenbank zu verbessern, müssen Sie sich darüber im Klaren sein, dass sich jede aktive Abfrageanforderung der Anwendung entweder in einem aktiven Zustand oder in einem Wartezustand befindet. Behalten Sie bei der Behandlung eines Leistungsproblems in Azure SQL-Datenbank das folgende Diagramm im Hinterkopf:

![Workload-Status](./media/sql-database-monitor-tune-overview/workload-states.png)

Ein Leistungsproblemen in einer Workload kann auf CPU-Konflikte (eine *ausführungsbezogene* Bedingung) oder auf einzelne Abfragen zurückzuführen sein, die auf etwas warten (eine *wartebezogene* Bedingung).

Ausführungsbezogene Probleme können folgende Ursachen haben:
- **Kompilierungsprobleme:** Der SQL-Abfrageoptimierer generiert möglicherweise aufgrund veralteter Statistiken, einer falschen Schätzung der zu verarbeitenden Zeilenanzahl oder einer falschen Schätzung des erforderlichen Arbeitsspeichers einen nicht optimalen Plan. Wenn Sie wissen, dass die Abfrage in der Vergangenheit oder in einer anderen Instanz (einer verwalteten Instanz oder einer SQL Server-Instanz) schneller ausgeführt wurde, untersuchen Sie die tatsächlichen Ausführungspläne auf mögliche Unterschiede. Versuchen Sie, Abfragehinweise anzuwenden oder Statistiken/Indizes neu zu erstellen, um den besseren Plan zu erhalten. Aktivieren Sie die automatische Plankorrektur in Azure SQL-Datenbank, um solche Probleme automatisch zu beheben.
- **Ausführungsprobleme:** Ist der Abfrageplan optimal, werden wahrscheinlich die Ressourcengrenzwerte der Datenbank (etwa beim Schreibdurchsatz für Protokolle) erreicht. Unter Umständen werden auch fragmentierte Indizes verwendet, die neu erstellt werden sollten. Ausführungsprobleme können auch auftreten, wenn eine große Anzahl gleichzeitiger Abfragen die gleichen Ressourcen benötigt. *Wartebezogene* Probleme hängen in der Regel mit Ausführungsproblemen zusammen, da die nicht effizient ausgeführten Abfragen wahrscheinlich auf Ressourcen warten.

Wartebezogene Probleme können folgende Ursachen haben:
- **Blockierung:** Möglicherweise wurden einige Objekte in der Datenbank für eine Abfrage gesperrt, während andere Abfragen versuchen, auf die gleichen Objekte zuzugreifen. Blockierende Abfragen können mithilfe von DMVs oder Überwachungstools identifiziert werden.
- **E/A-Probleme:** Abfragen warten möglicherweise darauf, dass Seiten in die Daten- oder Protokolldateien geschrieben werden. Überprüfen Sie in diesem Fall die Wartestatistik `INSTANCE_LOG_RATE_GOVERNOR`, `WRITE_LOG` oder `PAGEIOLATCH_*` in der DMV.
- **TempDB-Probleme:** Wenn die Workload temporäre Tabellen verwendet oder die Pläne TempDB-Überläufe enthalten, liegt bei den Anfragen unter Umständen ein Problem mit dem TempDB-Durchsatz vor. 
- **Arbeitsspeicherbezogene Probleme:** Wenn für die Workload nicht genügend Arbeitsspeicher zur Verfügung steht, sinkt unter Umständen die Seitenlebenserwartung, oder die Abfragen erhalten weniger Arbeitsspeicher als sie benötigen. In bestimmten Fällen können arbeitsspeicherbezogene Probleme durch die integrierte Intelligenz des Abfrageoptimierers behoben werden.
 
In den folgenden Abschnitten erfahren Sie, wie sich einige dieser Probleme identifizieren und beheben lassen.

## <a name="performance-problems-related-to-running"></a>Leistungsprobleme im Zusammenhang mit der Ausführung

Faustregel: Wenn die CPU-Auslastung konstant bei oder über 80 Prozent liegt, hängt das Leistungsproblem mit der Ausführung zusammen. Ein ausführungsbezogenes Problem kann durch unzureichende CPU-Ressourcen verursacht werden. Es kann aber auch auf eine der folgenden Bedingungen zurückzuführen sein:

- Zu viele ausgeführte Abfragen
- Zu viele kompilierte Abfragen
- Verwendung eines nicht optimalen Abfrageplans durch eine oder mehrere ausgeführte Abfragen

Liegt ein ausführungsbezogenes Problem vor, müssen Sie das Problem mithilfe einer oder mehrerer Methoden exakt identifizieren. Ausführungsbezogene Probleme werden im Allgemeinen mithilfe folgender Methoden identifiziert:

- Verwenden Sie das [Azure-Portal](sql-database-manage-after-migration.md#monitor-databases-using-the-azure-portal), um die prozentuale CPU-Auslastung zu überwachen.
- Verwenden Sie die folgenden [DMVs](sql-database-monitoring-with-dmvs.md):

  - Die DMV [sys.dm_db_resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) gibt die CPU-, E/A- und Arbeitsspeichernutzung für eine SQL-Datenbank zurück. Für alle 15 Sekunden ist eine Zeile enthalten, selbst wenn keine Aktivität in der Datenbank erfolgt ist. Historische Daten werden eine Stunde lang aufbewahrt.
  - Die DMV [sys.resource_stats](sql-database-monitoring-with-dmvs.md#monitor-resource-use) gibt CPU-Auslastungs- und Speicherdaten für Azure SQL-Datenbank zurück. Die Daten werden in Intervallen von fünf Minuten gesammelt und aggregiert.

> [!IMPORTANT]
> Informationen zum Behandeln von CPU-Auslastungsproblemen für T-SQL-Abfragen mit den DMVs „sys.dm_db_resource_stats“ und „sys.resource_stats“ finden Sie unter [Identifizieren von CPU-Leistungsproblemen](sql-database-monitoring-with-dmvs.md#identify-cpu-performance-issues).

### <a name="ParamSniffing"></a> Abfragen mit PSP-Problemen

Ein PSP-Problem (Parameter Sensitive Plan; parameterempfindlicher Plan) tritt auf, wenn der Abfrageoptimierer einen Abfrageausführungsplan generiert, der nur für einen bestimmten Parameterwert (oder eine Gruppe von Werten) optimal ist, und der zwischengespeicherte Plan somit für Parameterwerte späterer Ausführungen nicht optimal ist. Nicht optimale Pläne können zu Problemen mit der Abfrageleistung führen und den allgemeinen Workloaddurchsatz beeinträchtigen. 

Weitere Informationen zur Parameterermittlung und Abfrageverarbeitung finden Sie im [Handbuch zur Architektur der Abfrageverarbeitung](/sql/relational-databases/query-processing-architecture-guide#ParamSniffing).

PSP-Probleme können auf unterschiedliche Weise umgangen werden. Jede Problemumgehungen geht mit bestimmten Kompromissen und Nachteilen einher:

- Verwenden des Abfragehinweises [RECOMPILE](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query) bei jeder Abfrageausführung. Bei dieser Problemumgehung werden Kompilierungszeit und erhöhte CPU-Leistung für eine bessere Qualität des Abfrageplans eingebüßt. Die Option `RECOMPILE` kann bei Workloads, die einen hohen Durchsatz erfordern, häufig nicht verwendet werden.
- Verwenden Sie den Abfragehinweis [OPTION (OPTIMIZE FOR...)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query), um den tatsächlichen Parameterwert mit einem typischen Parameterwert zu überschreiben, der einen Plan erzeugt, der für die meisten möglichen Parameterwerte geeignet ist. Diese Option erfordert ein gutes Verständnis der optimalen Parameterwerte und zugehörigen Planmerkmale.
- Verwenden Sie den Abfragehinweis [OPTION (OPTIMIZE FOR UNKNOWN)](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query), um den tatsächlichen Parameterwert zu überschreiben und stattdessen den Durchschnittswert des Dichtevektors zu verwenden. Hierzu können Sie auch die eingehenden Parameterwerte in lokalen Variablen erfassen und dann anstatt der Parameter die lokalen Variablen innerhalb der Prädikate verwenden. Für diese Korrektur muss die durchschnittliche Dichte *gut genug* sein.
- Verwenden Sie den Abfragehinweis [DISABLE_PARAMETER_SNIFFING](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query), um die Parameterermittlung vollständig zu deaktivieren.
- Verwenden Sie den Abfragehinweis [KEEPFIXEDPLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query), um erneute Kompilierungen im Cache zu verhindern. Bei dieser Lösung wird davon ausgegangen, dass sich der ausreichende allgemeine Plan bereits im Cache befindet. Sie können auch automatische Statistikaktualisierungen deaktivieren, um die Wahrscheinlichkeit zu verringern, dass der gute Plan entfernt und ein neuer schlechter Plan kompiliert wird.
- Erzwingen Sie den Plan durch die explizite Verwendung des Abfragehinweises [USE PLAN](https://docs.microsoft.com/sql/t-sql/queries/hints-transact-sql-query), indem Sie die Abfrage neu schreiben und den Hinweis dem Abfragetext hinzufügen. Alternativ können Sie einen bestimmten Plan mithilfe des Abfragespeichers oder durch Aktivieren der [automatischen Optimierung](sql-database-automatic-tuning.md) festlegen.
- Ersetzen der Einzelprozedur durch eine geschachtelte Gruppe von Prozeduren, die jeweils basierend auf bedingter Logik und zugehörigen Parameterwerten verwendet werden können.
- Erstellen von Alternativen für die dynamische Zeichenfolgenausführung zur Definition einer statischen Prozedur.

Weitere Informationen zur Behebung von PSP-Problemen finden Sie in den folgenden Blogbeiträgen:

- [I Smell a Parameter!](https://blogs.msdn.microsoft.com/queryoptteam/2006/03/31/i-smell-a-parameter/) (Ich rieche einen Parameter)
- [Conor, dynamischer SQL-Code, Prozeduren und Planqualität für parametrisierte Abfragen](https://blogs.msdn.microsoft.com/conor_cunningham_msft/2009/06/03/conor-vs-dynamic-sql-vs-procedures-vs-plan-quality-for-parameterized-queries/)
- [Techniken zur SQL-Abfrageoptimierung in SQL Server: Parameterermittlung](https://www.sqlshack.com/query-optimization-techniques-in-sql-server-parameter-sniffing/)

### <a name="compile-activity-caused-by-improper-parameterization"></a>Durch nicht ordnungsgemäße Parametrisierung verursachte Kompilierungsaktivität

Wenn eine Abfrage Literale enthält, wird die Anweisung entweder automatisch durch die Datenbank-Engine parametrisiert, oder ein Benutzer parametrisiert die Anweisung explizit, um die Anzahl von Kompilierungen zu verringern. Eine hohe Anzahl von Kompilierungen für eine Abfrage mit dem gleichen Muster und unterschiedlichen Literalwerten kann zu einer hohen CPU-Auslastung führen. Wenn Sie eine Abfrage nur teilweise parametrisieren, sodass sie weiterhin Literale enthält, wird sie von der Datenbank-Engine nicht weiter parametrisiert.  

Hier sehen Sie ein Beispiel für eine teilweise parametrisierte Abfrage:

```sql
SELECT * 
FROM t1 JOIN t2 ON t1.c1 = t2.c1
WHERE t1.c1 = @p1 AND t2.c2 = '961C3970-0E54-4E8E-82B6-5545BE897F8F'
```

In diesem Beispiel nimmt `t1.c1` den Wert `@p1` an, aber `t2.c2` verwendet weiterhin die GUID als Literal. Wenn Sie in diesem Fall den Wert für `c2` ändern, wird die Abfrage als eine andere Abfrage behandelt und eine neue Kompilierung durchgeführt. In diesem Beispiel muss auch die GUID parametrisiert werden, um die Anzahl von Kompilierungen zu verringern.

Die folgende Abfrage zeigt die Anzahl von Abfragen nach Abfragehash, um zu ermitteln, ob eine Abfrage ordnungsgemäß parametrisiert ist:

```sql
SELECT  TOP 10  
  q.query_hash
  , count (distinct p.query_id ) AS number_of_distinct_query_ids
  , min(qt.query_sql_text) AS sampled_query_text
FROM sys.query_store_query_text AS qt
  JOIN sys.query_store_query AS q
     ON qt.query_text_id = q.query_text_id
  JOIN sys.query_store_plan AS p 
     ON q.query_id = p.query_id
  JOIN sys.query_store_runtime_stats AS rs 
     ON rs.plan_id = p.plan_id
  JOIN sys.query_store_runtime_stats_interval AS rsi
     ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id
WHERE
  rsi.start_time >= DATEADD(hour, -2, GETUTCDATE())
  AND query_parameterization_type_desc IN ('User', 'None')
GROUP BY q.query_hash
ORDER BY count (distinct p.query_id) DESC
```

### <a name="factors-that-affect-query-plan-changes"></a>Faktoren, die sich auf Abfrageplanänderungen auswirken

Die Neukompilierung eines Abfrageausführungsplans führt möglicherweise zu einem generierten Abfrageplan, der sich von dem ursprünglich zwischengespeicherten Plan unterscheidet. Ein vorhandener ursprünglicher Plan kann aus unterschiedlichen Gründen automatisch neu kompiliert werden:
- Von der Abfrage wird auf Änderungen im Schema verwiesen.
- Von der Abfrage wird auf Datenänderungen für die Tabellen verwiesen. 
- Abfragekontextoptionen wurden geändert.

Ein kompilierter Plan kann aus verschiedenen Gründen aus dem Cache entfernt werden:

- Neustart der Instanz
- Konfigurationsänderungen für die Datenbank
- Hohe Arbeitsspeicherauslastung
- Explizite Anforderungen zum Löschen des Caches

Bei Verwendung eines RECOMPILE-Hinweises wird der Plan nicht zwischengespeichert.

Eine Neukompilierung (oder eine erneute Kompilierung nach dem Entfernen aus dem Cache) kann trotzdem zur Generierung eines Abfrageausführungsplans führen, der mit dem ursprünglichen Plan identisch ist. Wenn sich der Plan vom vorherigen oder ursprünglichen Plan unterscheidet, lässt sich dies wahrscheinlich wie folgt erklären:

- **Geändertes physisches Design:** Neu erstellte Indizes können beispielsweise die Anforderungen einer Abfrage effektiver abdecken. Die neuen Indizes können in einer neuen Kompilierung verwendet werden, wenn der Abfrageoptimierer entscheidet, dass der neue Index besser geeignet ist als die Datenstruktur, die ursprünglich für die erste Version der Abfrageausführung gewählt wurde.  Jegliche physische Änderung an den referenzierten Objekten führt zur Kompilierzeit möglicherweise zu einer neuen Planauswahl.

- **Unterschiede zwischen Serverressourcen:** Wenn sich ein Plan in einem System vom Plan in einem anderen System unterscheidet, kann die Ressourcenverfügbarkeit (etwa die Anzahl verfügbarer Prozessoren) darüber entscheiden, welcher Plan generiert wird.  Wenn ein System also beispielsweise über mehr Prozessoren verfügt, wird möglicherweise ein paralleler Plan ausgewählt. 

- **Unterschiedliche Statistiken:** Möglicherweise haben sich die den referenzierten Objekten zugeordneten Statistiken geändert, oder sie unterscheiden sich wesentlich von den Statistiken des ursprünglichen Systems.  Wenn sich die Statistik ändert und eine Neukompilierung durchgeführt wird, verwendet der Abfrageoptimierer die Statistik ab der Änderung. Die überarbeiteten Datenverteilungen und Häufigkeiten der Statistik unterscheiden sich unter Umständen von denen der ursprünglichen Kompilierung.  Diese Änderungen werden zur Erstellung von Kardinalitätsschätzungen verwendet. (Bei *Kardinalitätsschätzungen* handelt es sich um die Anzahl von Zeilen, die voraussichtlich die logische Abfragestruktur durchlaufen). Änderungen an Kardinalitätsschätzungen können Sie ggf. dazu veranlassen, andere physische Operatoren und eine andere Vorgangsreihenfolge auszuwählen.  Selbst geringfügige Änderungen an Statistiken können zu einem anderen Abfrageausführungsplan führen.

- **Geänderter Datenbank-Kompatibilitätsgrad oder geänderte Version der Kardinalitätsschätzung:**  Bei Änderungen am Datenbank-Kompatibilitätsgrad können neue Strategien und Funktionen aktiviert werden, die möglicherweise zu einem anderen Abfrageausführungsplan führen.  Neben dem Datenbank-Kompatibilitätsgrad kann die Wahl des Abfrageausführungsplans zur Kompilierungszeit auch durch Deaktivieren oder Aktivieren des Ablaufverfolgungsflags 4199 oder durch Ändern des Zustands der datenbankbezogenen Konfiguration „QUERY_OPTIMIZER_HOTFIXES“ beeinflusst werden.  Auch die Ablaufverfolgungsflags 9481 (Legacy-CE erzwingen) und 2312 (Standard-CE erzwingen) wirken sich auf den Plan aus. 

### <a name="resolve-problem-queries-or-provide-more-resources"></a>Lösen von Abfrageproblemen oder Bereitstellen weiterer Ressourcen

Nachdem Sie das Problem identifiziert haben, können Sie die problematischen Abfragen optimieren oder die Computegröße oder Dienstebene upgraden, um die Kapazität Ihrer SQL-Datenbank zu erhöhen und so den CPU-Anforderungen gerecht zu werden. 

Weitere Informationen finden Sie unter [Skalieren von Einzeldatenbankressourcen in Azure SQL-Datenbank](sql-database-single-database-scale.md) sowie unter [Skalieren von Ressourcen für Pools für elastische Datenbanken in Azure SQL-Datenbank](sql-database-elastic-pool-scale.md). Informationen zum Skalieren einer verwalteten Instanz finden Sie unter [Ressourcenlimits der Dienstebene](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

### <a name="performance-problems-caused-by-increased-workload-volume"></a>Leistungsprobleme aufgrund eines erhöhten Workloadaufkommens

Eine erhöhte CPU-Auslastung kann auf eine Zunahme des Anwendungsdatenverkehrs und des Workloadaufkommens zurückzuführen sein. Dieses Problem muss allerdings sorgfältig diagnostiziert werden. Beantworten Sie im Falle eines Problems mit hoher CPU-Auslastung die folgenden Fragen, um zu ermitteln, ob die höhere Auslastung auf Veränderungen beim Workloadaufkommen zurückzuführen ist:

- Sind die Abfragen von der Anwendung die Ursache für das Problem mit hoher CPU-Auslastung?
- Für die Abfragen mit der höchsten CPU-Auslastung, die Sie identifizieren können:

   - Wurden der gleichen Abfrage mehrere Ausführungspläne zugeordnet? Falls ja: Warum?
   - Waren die Ausführungszeiten bei Abfragen mit dem gleichen Ausführungsplan konsistent? Hat sich die Ausführungsanzahl erhöht? Falls ja, führt die Workloadzunahme wahrscheinlich zu Leistungsproblemen.

Zusammenfassend lässt sich sagen: Wenn der Abfrageausführungsplan nicht anders ausgeführt wurde, die CPU-Auslastung aber zusammen mit der Anzahl von Ausführungen zugenommen hat, hängt das Leistungsproblem wahrscheinlich mit einer Workloadzunahme zusammen.

Es ist nicht immer einfach, eine Änderung des Workloadaufkommens zu ermitteln, die einem CPU-Problem zugrunde liegt. Beachten Sie folgende Faktoren: 

- **Geänderte Ressourcennutzung:** Nehmen wir beispielsweise an, in einem Szenario hat sich die CPU-Auslastung für einen längeren Zeitraum auf 80 Prozent erhöht.  Die CPU-Auslastung allein bedeutet jedoch nicht, dass sich das Workloadaufkommen geändert hat. Auch Regressionen im Abfrageausführungsplans und eine geänderte Datenverteilung können zu einer höheren Ressourcennutzung beitragen, selbst wenn die Anwendung die gleiche Workload ausführt.

- **Auftauchen einer neuen Abfrage:** Eine Anwendung kann zu unterschiedlichen Zeiten eine neue Gruppe von Abfragen auslösen.

- **Erhöhung oder Verringerung der Anforderungsanzahl:** Dieses Szenario ist der offensichtlichste Maßstab einer Workload. Die Anzahl der Abfragen entspricht nicht immer einer gesteigerten Ressourcennutzung. Diese Metrik ist dennoch ein wichtiges Signal, falls sich andere Faktoren nicht geändert haben.

## <a name="waiting-related-performance-problems"></a>Wartebezogene Leistungsprobleme 

Wenn Sie sicher sind, dass Ihr Leistungsproblem nicht mit hoher CPU-Auslastung oder mit der Ausführung zusammenhängt, ist Ihr Problem wartebedingt. Das bedeutet: Ihre CPU-Ressourcen werden nicht effizient genutzt, da die CPU auf eine andere Ressource wartet. Ermitteln Sie in diesem Fall, worauf Ihre CPU-Ressourcen warten. 

Im Anschluss finden Sie gängige Methoden zum Anzeigen der wichtigsten Kategorien von Wartetypen:

- Verwenden Sie den [Abfragespeicher](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store), um Wartestatistiken für die einzelnen Abfragen im Zeitverlauf zu suchen. Im Abfragedatenspeicher sind Wartetypen mit Wartekategorien kombiniert. Die Zuordnung von Wartekategorien zu Wartetypen finden Sie unter [sys.query_store_wait_stats (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql#wait-categories-mapping-table).
- Verwenden Sie [sys.dm_db_wait_stats (Azure SQL-Datenbank)](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database), um Informationen zu allen Wartezeiten für Threads zurückzugeben, die während des Vorgangs ausgeführt wurden. Sie können diese aggregierte Ansicht verwenden, um Leistungsprobleme mit Azure SQL-Datenbank sowie mit bestimmten Abfragen und Batches zu diagnostizieren.
- Verwenden Sie [sys.dm_os_waiting_tasks (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql), um Informationen zur Warteschlange von Aufgaben zurückzugeben, die auf eine Ressource warten.

In Szenarien mit hoher CPU-Auslastung wird die CPU-Auslastung durch den Abfragespeicher und die Wartestatistik in folgenden Fällen möglicherweise nicht korrekt wiedergegeben:

- Abfragen mit hoher CPU-Auslastung werden noch ausgeführt.
- Die Abfragen mit hoher CPU-Auslastung wurden parallel zu einem Failover ausgeführt.

DMVs, die den Abfragespeicher und die Wartestatistik nachverfolgen, zeigen nur Ergebnisse für erfolgreich abgeschlossene Abfragen sowie für Abfragen an, bei denen ein Timeout aufgetreten ist. Sie zeigen keine Daten für aktuell ausgeführte Anweisungen an, bis die Anweisungen abgeschlossen sind. Verwenden Sie die dynamische Verwaltungssicht [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql), um aktuell ausgeführte Abfragen und die Zeit des zugeordneten Workers anzuzeigen.

Das Diagramm am Anfang dieses Artikels gibt Aufschluss über gängigsten Wartetypen:

- Sperren (Blockieren)
- E/A
- Konflikte im Zusammenhang mit TempDB
- Wartetyp „Speicherzuweisung“

> [!IMPORTANT]
> Eine Reihe von T-SQL-Abfragen, die DMVs verwenden, um wartebezogene Probleme zu behandeln, finden Sie unter:
>
> - [Identifizieren von Problemen mit der E/A-Leistung](sql-database-monitoring-with-dmvs.md#identify-io-performance-issues)
> - [Identifizieren von Problemen mit Wartevorgängen aufgrund von Speicherzuweisungen](sql-database-monitoring-with-dmvs.md#identify-memory-grant-wait-performance-issues)
> - [TigerToolbox: Wartevorgänge und Latches](https://github.com/Microsoft/tigertoolbox/tree/master/Waits-and-Latches)
> - [TigerToolbox: usp_whatsup](https://github.com/Microsoft/tigertoolbox/tree/master/usp_WhatsUp)

## <a name="improve-database-performance-with-more-resources"></a>Verbessern der Datenbankleistung mit mehr Ressourcen

Wenn sich die Leistung Ihrer Datenbank anderweitig nicht verbessern lässt, können Sie die in Azure SQL-Datenbank verfügbare Ressourcenmenge ändern. Weisen Sie weitere Ressourcen zu, indem Sie die [DTU-Dienstebene](sql-database-service-tiers-dtu.md) einer einzelnen Datenbank ändern. Sie können auch jederzeit die eDTUs eines Pools für elastische Datenbanken ändern. Alternativ können Sie bei Verwendung des [vCore-basierten Kaufmodells](sql-database-service-tiers-vcore.md) entweder die Dienstebene ändern oder die der Datenbank zugeordneten Ressourcen erhöhen.

Bei Einzeldatenbanken können Sie bedarfsgesteuert die [Dienstebenen oder Computeressourcen ändern](sql-database-single-database-scale.md), um die Datenbankleistung zu steigern. Ziehen Sie bei mehreren Datenbanken [Pools für elastische Datenbanken](sql-database-elastic-pool-guidance.md) in Betracht, um Ressourcen automatisch zu skalieren.

## <a name="tune-and-refactor-application-or-database-code"></a>Optimieren und Ändern von Anwendungs- oder Datenbankcode

Sie können den Anwendungscode für die Datenbank optimieren, Indizes ändern, Pläne erzwingen oder Hinweise verwenden, um die Datenbank manuell an Ihre Workload anzupassen. Informationen zum manuellen Optimieren und Überarbeiten des Codes finden Sie im [Leitfaden zur Leistungsoptimierung](sql-database-performance-guidance.md).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Aktivierung der automatischen Optimierung in Azure SQL-Datenbank und zur Verwendung für die vollständige Verwaltung Ihrer Workload finden Sie unter [Aktivieren der automatischen Optimierung zum Überwachen von Abfragen und Verbessern der Workloadleistung](sql-database-automatic-tuning-enable.md).
- Informationen zur manuellen Optimierung finden Sie unter [Suchen und Anwenden von Empfehlungen zur Leistung](sql-database-advisor-portal.md). Wenden Sie die Empfehlungen zur Verbesserung der Abfrageleistung manuell an.
- Ändern Sie [Dienstebenen für Azure SQL-Datenbank](sql-database-performance-guidance.md), um die in Ihrer Datenbank zur Verfügung stehenden Ressourcen zu ändern.
