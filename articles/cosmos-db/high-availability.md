---
title: Hochverfügbarkeit in Azure Cosmos DB
description: Dieser Artikel beschreibt, wie Azure Cosmos DB Hochverfügbarkeit bietet.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 928c943e21e7d00b87ac1e506b98d47107ac4348
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508554"
---
# <a name="high-availability-with-azure-cosmos-db"></a>Hochverfügbarkeit mit Azure Cosmos DB

Azure Cosmos DB repliziert Ihre Daten transparent in allen Azure-Regionen, die Ihrem Cosmos-Konto zugewiesen sind. Cosmos DB verwendet mehrere Redundanzebenen für Ihre Daten, wie in der folgenden Abbildung dargestellt:

![Physische Partitionierung](./media/high-availability/cosmosdb-data-redundancy.png)

- Die Daten in Cosmos-Containern sind [horizontal partitioniert](partitioning-overview.md).

- In jeder Region wird jede Partition durch einen Replikatsatz geschützt. Dabei werden alle Schreibvorgänge repliziert, und die Mehrheit der Replikate führt ein dauerhaftes Commit für sie aus. Replikate werden auf 10 bis 20 Fehlerdomänen verteilt.

- Jede Partition wird in allen Regionen repliziert. Jede Region enthält alle Datenpartitionen eines Cosmos-Containers und kann Schreibvorgänge zulassen sowie Lesevorgänge verarbeiten.  

Wenn Ihr Cosmos-Konto über *N* Azure-Regionen verteilt ist, gibt es mindestens *N* x 4 Kopien von allen Ihren Daten. Neben dem Zugriff mit geringer Latenz auf Ihre Daten und der Skalierung des Schreib- und Lesedurchsatzes in den Regionen, die Ihrem Cosmos-Konto zugeordnet sind, verbessert die Bereitstellung mehrerer Regionen (größer als *N*) die Verfügbarkeit noch mehr.  

## <a name="slas-for-availability"></a>SLAs für Verfügbarkeit

Als global verteilte Datenbank bietet Cosmos DB umfangreiche SLAs, die Durchsatz, Latenz im 99. Perzentil, Konsistenz und Hochverfügbarkeit umfassen. In der Tabelle unten sind die Garantieangaben für die Hochverfügbarkeit aufgeführt, die von Cosmos DB für Konten mit einer oder mehreren Regionen gelten. Für Hochverfügbarkeit konfigurieren Sie Ihre Cosmos-Konten immer mit mehreren Schreibregionen.

|Vorgangsart  | Eine Region |Mehrere Regionen (eine Schreibregion)|Mehrere Regionen (mehrere Schreibregionen) |
|---------|---------|---------|-------|
|Schreibvorgänge    | 99,99    |99,99   |99,999|
|Lesevorgänge     | 99,99    |99,999  |99,999|

> [!NOTE]
> In der Praxis ist die tatsächliche Verfügbarkeit von Schreibvorgängen für die Modelle „Begrenzte Veraltung“, „Sitzung“, „Präfixkonsistenz“ und „Letztliche Konsistenz“ deutlich höher als die veröffentlichten SLAs. Die tatsächliche Verfügbarkeit von Lesevorgängen ist für alle Konsistenzebenen deutlich höher als die veröffentlichten SLAs.

## <a name="high-availability-with-cosmos-db-in-the-event-of-regional-outages"></a>Hochverfügbarkeit mit Cosmos DB bei regionalen Ausfällen

Regionale Ausfälle sind keine Seltenheit. Deswegen stellt Azure Cosmos DB sicher, dass Ihre Datenbank immer hochverfügbar ist. Im Folgenden wird das Verhalten von Cosmos DB – abhängig von der Konfiguration Ihres Cosmos-Kontos – bei einem Ausfall beschrieben:

- Bevor ein Schreibvorgang dem Client gegenüber bestätigt wird, wird bei Cosmos DB von einem Quorum von Replikaten in der Region, die die Schreibvorgänge akzeptiert, ein dauerhaftes Commit ausgeführt.

- Konten mit mehreren Regionen, die mit mehreren Schreibregionen konfiguriert sind, sind sowohl für Schreib- als auch für Lesevorgänge hoch verfügbar. Regionale Failover werden sofort ausgeführt und erfordern keine Änderungen von der Anwendung.

- **Konten mit mehreren Regionen mit einer einzelnen Schreibregion (Ausfall einer Schreibregion):** Diese Konten bleiben bei Ausfall einer Schreibregion hochverfügbar für Lesevorgänge. Für Schreibvorgänge müssen Sie jedoch in Ihrem Cosmos-Konto die Option **Automatisches Failover aktivieren** auswählen, damit ein Failover für die betreffende Region in eine andere Region ausgeführt werden kann. Das Failover erfolgt in der Reihenfolge der von Ihnen angegebenen Priorisierung der Regionen. Wenn die betroffene Region wieder online ist, werden die nicht replizierten Daten, die sich während des Ausfalls in der betroffenen Schreibregion befanden, über den [Konfliktfeed](how-to-manage-conflicts.md#read-from-conflict-feed) verfügbar gemacht. Anwendungen können den Konfliktfeed lesen, die Konflikte auf Grundlage der anwendungsspezifischen Logik lösen und aktualisierte Daten nach Bedarf in den Cosmos-Container zurückschreiben. Wenn die vom Ausfall betroffene Schreibregion wiederhergestellt ist, steht sie automatisch als Leseregion zur Verfügung. Sie können ein manuelles Failover aufrufen und die betreffende Region als Schreibregion konfigurieren. Sie können mithilfe der [Azure-Befehlszeilenschnittstelle oder des Azure-Portals](how-to-manage-database-account.md#manual-failover) wieder ein manuelles Failover ausführen. Es gibt **keinerlei Daten- oder Verfügbarkeitsverluste** – weder vor oder nach dem manuellen Failover noch währenddessen. Ihre Anwendung ist weiterhin hochverfügbar. 

- **Konten mit mehreren Regionen mit einer einzelnen Schreibregion (Ausfall einer Leseregion):** Diese Konten bleiben bei Ausfall einer Leseregion hochverfügbar für Lese- und Schreibvorgänge. Die betreffende Region wird automatisch von der Schreibregion getrennt und als offline gekennzeichnet. Die [Cosmos DB SDKs](sql-api-sdk-dotnet.md) leiten Leseaufrufe an die nächste verfügbare Region in der Liste der bevorzugten Regionen weiter. Wenn keine der Regionen in der Liste verfügbar ist, wird für die Aufrufe automatisch ein Fallback zur aktuellen Schreibregion durchgeführt. Zur Verarbeitung des Ausfalls einer Leseregion sind keine Änderungen an Ihrem Anwendungscode erforderlich. Wenn die betreffende Region wieder online ist, wird die vom Ausfall betroffene Leseregion automatisch mit der aktuellen Schreibregion synchronisiert. Sie steht dann wieder für die Verarbeitung von Leseanforderungen zur Verfügung. Nachfolgende Lesevorgänge werden an die wiederhergestellte Region weitergeleitet, ohne dass Änderungen an Ihrem Anwendungscode erforderlich sind. Sowohl beim Failover als auch beim erneuten Verknüpfen einer zuvor ausgefallene Region gelten weiterhin die Lesekonsistenzgarantien von Cosmos DB.

- Konten mit einer Region sind nach einem regionalen Ausfall ggf. nicht mehr verfügbar. Es wird immer empfohlen, **mindestens zwei Regionen** (idealerweise mindestens zwei Schreibregionen) für Ihr Cosmos-Konto einzurichten, um die permanente Hochverfügbarkeit sicherzustellen.

- Auch in dem seltenen und unglücklichen Fall, in dem die Azure-Region dauerhaft nicht mehr wiederhergestellt werden kann, kommt es nicht zu einem Datenverlust, wenn für Ihr Cosmos-Konto mit mehreren Regionen die Standardkonsistenzebene *Sicher* konfiguriert ist. Im Falle einer dauerhaft nicht mehr wiederherstellbaren Schreibregion ist das Fenster für potenzielle Datenverluste für die Cosmos-Konten mit mehreren Regionen, für die eine Konsistenz vom Typ „Begrenzte Veraltung“ konfiguriert ist, auf das Fenster der Veraltung (*K* oder *T*) begrenzt. Für die Konsistenzebenen „Sitzung“, „Präfixkonsistenz“ und „Letztlich“ ist das Fenster für potenzielle Datenverluste auf maximal fünf Sekunden begrenzt. 

## <a name="availability-zone-support"></a>Unterstützung von Verfügbarkeitszonen

Azure Cosmos DB ist ein global verteilter Datenbankdienst mit mehreren Mastern und bietet Hochverfügbarkeit sowie Resilienz bei regionalen Ausfällen. Neben regionsübergreifender Resilienz kann nun beim Auswählen einer Region, die Ihrer Azure Cosmos-Datenbank zugeordnet werden soll, auch **Zonenredundanz** aktiviert werden. 

Durch die Unterstützung von Verfügbarkeitszonen stellt Azure Cosmos DB sicher, dass Replikate in mehreren Zonen einer bestimmten Region platziert werden, um Hochverfügbarkeit und Resilienz bei Zonenausfällen zu gewährleisten. Wartezeit und andere SLAs bleiben in dieser Konfiguration unverändert. Sollte eine einzelne Zone ausfallen, sorgt die Zonenredundanz für vollständige Dauerhaftigkeit (RPO: 0) und Verfügbarkeit (RTO: 0) der Daten. 

Zonenredundanz ist eine *Ergänzungsfunktion* für die [Multimasterreplikation](how-to-multi-master.md). Zonenredundanz allein reicht jedoch nicht aus, um regionale Resilienz zu erreichen. Daher empfiehlt es sich beispielsweise bei regionalen Ausfällen oder bei regionsübergreifenden Zugriffen mit geringer Wartezeit, neben der Zonenredundanz auch mehrere Schreibregionen zu verwenden. 

Wenn Sie für Ihr Azure Cosmos-Konto Schreibvorgänge in mehreren Regionen konfigurieren, können Sie die Zonenredundanz ohne zusätzliche Kosten aktivieren. Lesen Sie andernfalls weiter unten den Hinweis zu den Preisen für die Unterstützung der Zonenredundanz. Sie können die Zonenredundanz für eine vorhandene Region Ihres Azure Cosmos-Kontos aktivieren, indem Sie die Region entfernen und anschließend mit aktivierter Zonenredundanz wieder hinzufügen.

Dieses Feature steht in folgenden Azure-Regionen zur Verfügung:

* UK, Süden
* Asien, Südosten 
* East US
* USA (Ost) 2 
* USA (Mitte)

> [!NOTE] 
> Wenn Sie Verfügbarkeitszonen für ein Azure Cosmos-Konto mit einer einzelnen Region aktivieren, fallen die gleichen Kosten an wie beim Hinzufügen einer weiteren Region zu Ihrem Konto. Ausführliche Informationen zu den Preisen finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/cosmos-db/) sowie im Artikel [Optimieren der Kosten bei mehreren Regionen in Azure Cosmos DB](optimize-cost-regions.md). 

In der folgenden Tabelle ist die Hochverfügbarkeitsfunktion verschiedener Kontokonfigurationen zusammengefasst: 

|KPI  |Einzelne Region ohne Verfügbarkeitszonen (keine VZ)  |Einzelne Region mit Verfügbarkeitszonen (VZ)  |Schreibvorgänge in mehreren Regionen mit Verfügbarkeitszonen (VZ, zwei Regionen) – empfohlene Einstellung |
|---------|---------|---------|---------|
|Schreibverfügbarkeit (SLA)     |   99,99 %      |    99,99 %     |  99,999%  |
|Leseverfügbarkeit (SLA)   |   99,99 %      |   99,99 %      |  99,999%       |
|Preis  |  Preis für einzelne Region |  Preis für einzelne Region (Verfügbarkeitszone) |  Preis für mehrere Regionen       |
|Zonenfehler: Datenverlust   |  Datenverlust  |   Kein Datenverlust |   Kein Datenverlust  |
|Zonenfehler: Verfügbarkeit |  Verlust der Verfügbarkeit  | Kein Verlust der Verfügbarkeit  |  Kein Verlust der Verfügbarkeit  |
|Wartezeit beim Lesen    |  Regionsübergreifend    |   Regionsübergreifend   |    Niedrig  |
|Wartezeit beim Schreiben    |   Regionsübergreifend   |  Regionsübergreifend    |   Niedrig   |
|Regionaler Ausfall: Datenverlust    |   Datenverlust      |  Datenverlust       |   Datenverlust <br/><br/> Bei Verwendung der Konsistenz „Begrenzte Veraltung“ mit mehreren Mastern und mehreren Regionen sind Datenverluste auf die begrenzte Veraltung beschränkt, die für Ihr Konto konfiguriert ist. <br/><br/> Datenverluste bei regionalen Ausfällen lassen sich durch Konfigurieren einer hohen Konsistenz mit mehreren Regionen vermeiden. Bei dieser Option müssen Kompromisse bei der Verfügbarkeit und Leistung eingegangen werden.      |
|Regionaler Ausfall: Verfügbarkeit  |  Verlust der Verfügbarkeit       |  Verlust der Verfügbarkeit       |  Kein Verlust der Verfügbarkeit  |
|Throughput    |  X RU/s (bereitgestellter Durchsatz)      |  X RU/s (bereitgestellter Durchsatz)       |  2X RU/s (bereitgestellter Durchsatz) <br/><br/> Bei diesem Konfigurationsmodus wird im Vergleich zu einer einzelnen Region mit Verfügbarkeitszonen die doppelte Menge an Durchsatz benötigt, da zwei Regionen vorhanden sind.   |

> [!NOTE] 
> Für die Unterstützung von Verfügbarkeitszonen müssen für das Azure Cosmos DB-Konto Multimaster-Schreibvorgänge bzw. Schreibvorgänge für mehrere Regionen aktiviert sein. 

Sie können die Zonenredundanz aktivieren, wenn Sie eine Region zu neuen oder bereits vorhandenen Azure Cosmos-Konten hinzufügen. Aktuell kann die Zonenredundanz nur über PowerShell oder mit dem Azure-Portal, PowerShell und Azure Resource Manager-Vorlagen aktiviert werden. Wenn Sie die Zonenredundanz für Ihr Azure Cosmos-Konto aktivieren möchten, müssen Sie das Flag `isZoneRedundant` für einen bestimmten Standort auf `true` festlegen. Dieses Flag kann in der Standorteigenschaft festgelegt werden. Der folgende PowerShell-Codeausschnitt aktiviert beispielsweise die Zonenredundanz für die Region „Asien, Südosten“:

```powershell
$locations = @( 
    @{ "locationName"="Southeast Asia"; "failoverPriority"=0; "isZoneRedundant"= "true" }, 
    @{ "locationName"="East US"; "failoverPriority"=1 } 
) 
```

Sie können Verfügbarkeitszonen aktivieren, indem Sie beim Erstellen eines Azure Cosmos-Kontos das Azure-Portal verwenden. Stellen Sie beim Erstellen eines Kontos sicher, dass Sie **Georedundanz** und **Schreibvorgänge in mehreren Regionen** aktivieren und eine Region auswählen, in der Verfügbarkeitszonen unterstützt werden: 

![Aktivieren von Verfügbarkeitszonen mit dem Azure-Portal](./media/high-availability/enable-availability-zones-using-portal.png) 

## <a name="building-highly-available-applications"></a>Erstellen hochverfügbarer Anwendungen

- Um Hochverfügbarkeit von Schreib- und Lesevorgängen zu garantieren, konfigurieren Sie Ihr Cosmos-Konto so, dass es mindestens zwei Regionen mit mehreren Schreibregionen umfasst. Diese Konfiguration sorgt für höchste Verfügbarkeit, geringstmögliche Latenz und optimale Skalierbarkeit für Lese- und Schreibvorgänge und wird durch SLAs unterstützt. Weitere Informationen finden Sie unter [Konfigurieren eines Cosmos-Kontos mit mehreren Schreibregionen](tutorial-global-distribution-sql-api.md).

- Aktivieren Sie für Cosmos-Konten mit mehreren Regionen, die mit einer einzelnen Schreibregion konfiguriert sind, [„Automatisches Failover“ über die Azure-Befehlszeilenschnittstelle oder das Azure-Portal](how-to-manage-database-account.md#automatic-failover). Wenn Sie das automatische Failover aktivieren, führt Cosmos DB bei einem regionalen Ausfall ein automatisches Failover für Ihr Konto aus.  

- Selbst wenn Ihr Cosmos-Konto hochverfügbar ist, ist Ihre Anwendung möglicherweise nicht richtig dafür konzipiert, hochverfügbar zu bleiben. Um die End-to-End-Hochverfügbarkeit Ihrer Anwendung zu testen, rufen Sie im Rahmen Ihrer Anwendungs- oder Notfallwiederherstellungstests regelmäßig über die [Azure-Befehlszeilenschnittstelle oder das Azure-Portal ein manuelles Failover](how-to-manage-database-account.md#manual-failover) auf.

- Bei einer global verteilten Datenbankumgebung besteht eine direkte Beziehung zwischen der Konsistenzebene und der Datendauerhaftigkeit im Falle eines regionsweiten Ausfalls. Wenn Sie Ihren Plan für die Geschäftskontinuität entwickeln, müssen Sie wissen, wie viel Zeit maximal vergehen darf, bis die Anwendung nach einer Störung vollständig wiederhergestellt ist. Die Zeit, die für die vollständige Wiederherstellung einer Anwendung erforderlich ist, wird als RTO (Recovery Time Objective) bezeichnet. Sie müssen auch wissen, über welchen Zeitraum kürzlich durchgeführte Datenupdates maximal verloren gehen dürfen, wenn die Anwendung nach einer Störung wiederhergestellt wird. Der Zeitraum der Updates, der verloren gehen darf, wird als RPO (Recovery Point Objective) bezeichnet. Informationen zum Anzeigen von RPO und RTO für Azure Cosmos DB finden Sie unter [Kompromisse in Bezug auf Konsistenz, Verfügbarkeit und Leistung](consistency-levels-tradeoffs.md#rto).

## <a name="next-steps"></a>Nächste Schritte

Als Nächstes können Sie die folgenden Artikel lesen:

* [Kompromisse in Bezug auf Verfügbarkeit und Leistung für verschiedene Konsistenzebenen](consistency-levels-tradeoffs.md)
* [Globales Skalieren von bereitgestelltem Durchsatz](scaling-throughput.md)
* [Globale Verteilung: Hintergrundinformationen](global-dist-under-the-hood.md)
* [Konsistenzebenen in Azure Cosmos DB](consistency-levels.md)
* [Konfigurieren eines Cosmos-Kontos mit mehreren Schreibregionen](how-to-multi-master.md)
