---
title: Global verteilter transaktionaler und analytischer Speicher für Azure Cosmos-Container
description: Erfahren Sie mehr über transaktionalen und analytischen Speicher und die Konfigurationsoptionen für Azure Cosmos-Container.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/30/2019
ms.reviewer: sngun
ms.openlocfilehash: 22bb36e3b22f65bbf9922bd31e4b2e041cdb8979
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2019
ms.locfileid: "73601232"
---
# <a name="globally-distributed-transactional-and-analytical-storage-for-azure-cosmos-containers"></a>Global verteilter transaktionaler und analytischer Speicher für Azure Cosmos-Container

Ein Azure Cosmos-Container wird intern durch zwei Speicher-Engines unterstützt: eine transaktionale Speicher-Engine und eine aktualisierbare analytische Speicher-Engine. Beide Speicher-Engines sind protokollstrukturiert und schreiboptimiert, um schnellere Aktualisierungen zu ermöglichen. Allerdings werden beide Speichertypen unterschiedlich codiert:

* Die **transaktionale Speicher-Engine** verfügt über ein zeilenorientiertes Format, das schnelle Lese- und Abfragetransaktionen unterstützt.

* Die **analytische Speicher-Engine** verfügt über ein spaltenorientiertes Format, das schnelle analytische Abfragen und Scans unterstützt.

![Speicher-Engines und Azure Cosmos DB-API-Zuordnung](./media/globally-distributed-transactional-analytical-storage/storage-engines-api-mapping.png)

Die transaktionale Speicher-Engine wird durch lokale SSDs unterstützt, während für den analytischen Speicher kostengünstiger SSD-Speicher außerhalb des Clusters verwendet wird. Die folgende Tabelle verdeutlicht die wichtigsten Unterschiede zwischen transaktionalem und analytischem Speicher.


|Feature  |Transaktionaler Speicher  |Analytischer Speicher |
|---------|---------|---------|
|Maximaler Speicher pro Azure Cosmos-Container |   Unbegrenzt      |    Unbegrenzt     |
|Maximaler Speicher pro logischem Partitionsschlüssel   |   10 GB      |   Unbegrenzt      |
|Speichercodierung  |   Zeilenorientiert, verwendet ein internes Format   |   Spaltenorientiert, verwendet das Apache Parquet-Format |
|Ort der Speicherung |   Replizierter Speicher, der durch lokale/clusterinterne SSDs unterstützt wird |  Replizierter Speicher, der durch kostengünstige Remote-SSDs bzw. clusterexterne SSDs unterstützt wird       |
|Dauerhaftigkeit  |    99,99999 (7–9 s)     |  99,99999 (7–9 s)       |
|APIs für den Datenzugriff  |   SQL, MongoDB, Cassandra, Gremlin, Tabellen und etcd       | Apache Spark         |
|Aufbewahrung (Gültigkeitsdauer bzw. TTL)   |  Richtliniengesteuert, mit der `DefaultTimeToLive`-Eigenschaft im Azure Cosmos-Container konfiguriert       |   Richtliniengesteuert, mit der `ColumnStoreTimeToLive`-Eigenschaft im Azure Cosmos-Container konfiguriert      |
|Preis pro GB    |   Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/cosmos-db/)     |   Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/cosmos-db/)        |
|Preis für Speichertransaktionen    |  Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/cosmos-db/)         |   Informieren Sie sich auf der [Preisseite](https://azure.microsoft.com/pricing/details/cosmos-db/)        |

## <a name="benefits-of-transactional-and-analytical-storage"></a>Vorteile des transaktionalen und analytischen Speichers

### <a name="no-etl-operations"></a>Keine ETL-Vorgänge

Herkömmliche Analysepipelines sind komplex und umfassen mehrere Stufen. Jede Stufe erfordert ETL-Vorgänge (Extrahieren, Transformieren und Laden), die an die Compute- und Speicherebene gerichtet sind und davon ausgehen. So entstehen komplexe Datenverarbeitungsarchitekturen, die nicht nur hohe Kosten für mehrere Speicher- und Computestufen, sondern auch lange Wartezeiten verursachen, weil sehr große Datenmengen zwischen verschiedenen Speicher- und Computestufen übertragen werden müssen.  

Die Ausführung von ETL-Vorgängen mit Azure Cosmos DB verursacht keinen Mehraufwand. Jeder Azure Cosmos-Container wird sowohl von transaktionalen als auch von analytischen Speicher-Engines unterstützt. Die Datenübertragung zwischen der transaktionalen und analytischen Speicher-Engine erfolgt innerhalb der Datenbank-Engine und ohne Netzwerkhops. Die resultierende Wartezeit und die Kosten fallen im Vergleich zu herkömmlichen Analyselösungen wesentlich niedriger aus. Außerdem erhalten Sie ein einzelnes, global verteiltes Speichersystem sowohl für transaktionale als auch für analytische Workloads.  

### <a name="store-multiple-versions-update-and-query-the-analytical-storage"></a>Speichern mehrerer Versionen, Aktualisieren und Abfragen des analytischen Speichers

Der analytische Speicher ist vollständig aktualisierbar und enthält den kompletten Versionsverlauf aller Transaktionsaktualisierungen im Azure Cosmos-Container.

Alle Aktualisierungen des transaktionalen Speichers sind garantiert innerhalb von 30 Sekunden für den analytischen Speicher erkennbar. 

### <a name="globally-distributed-multi-master-analytical-storage"></a>Global verteilter, analytischer Multimasterspeicher

Wenn der Bereich Ihres Azure Cosmos-Kontos auf eine einzelne Region beschränkt ist, sind die (im transaktionalen und analytischen Speicher enthaltenen) Daten in den Containern ebenfalls auf eine einzelne Region beschränkt. Wenn das Azure Cosmos-Konto hingegen global verteilt ist, sind auch die in den Containern gespeicherten Daten global verteilt.

Bei Azure Cosmos-Konten, die mit mehreren Schreibregionen konfiguriert sind, werden Schreibvorgänge in den Container (sowohl in den transaktionalen als auch in den analytischen Speicher) immer in der lokalen Region und daher schnell ausgeführt.

Bei Azure Cosmos-Konten für eine einzelne oder mehrere Regionen werden sowohl transaktionale als auch analytische Lese-/Abfragevorgänge lokal in der angegebenen Region ausgeführt. Dies gilt unabhängig davon, ob es sich um eine einzelne oder um mehrere Schreibregionen handelt (auch als Einzelmaster- bzw. Multimasterszenario bezeichnet).

### <a name="performance-isolation-between-transactional-and-analytical-workloads"></a>Leistungsisolation zwischen transaktionalen und analytischen Workloads

In der jeweiligen Region werden die transaktionalen Workloads für den transaktionalen/zeilenbasierten Containerspeicher ausgeführt. Analytische Workloads werden hingegen für den analytischen/spaltenbasierten Containerspeicher ausgeführt. Die beiden Speicher-Engines arbeiten unabhängig voneinander, wobei die Leistung zwischen den Workloads streng isoliert ist.

Die transaktionalen Workloads nutzen den bereitgestellten Durchsatz (RUs). Der Durchsatz analytischer Workloads basiert im Gegensatz zu transaktionalen Workloads auf dem tatsächlichen Verbrauch. Analytische Workloads nutzen Ressourcen je nach Bedarf.

## <a name="next-steps"></a>Nächste Schritte

* [Gültigkeitsdauer in Azure Cosmos DB](time-to-live.md)
