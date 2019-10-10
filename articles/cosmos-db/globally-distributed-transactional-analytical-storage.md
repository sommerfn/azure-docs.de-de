---
title: Global verteilter transaktionaler und analytischer Speicher für Azure Cosmos-Container
description: Erfahren Sie mehr über transaktionalen und analytischen Speicher und die Konfigurationsoptionen für Azure Cosmos-Container.
author: rimman
ms.author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/30/2019
ms.reviewer: sngun
ms.openlocfilehash: 27ca2102ee95273fbedd1a870e57d2ae3318e879
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703623"
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
|APIs für den Datenzugriff  |   SQL, MongoDB, Cassandra, Gremlin, Tables und etcd       | Apache Spark         |
|Aufbewahrung (Gültigkeitsdauer bzw. TTL)   |  Richtliniengesteuert, mit der `DefaultTimeToLive`-Eigenschaft im Azure Cosmos-Container konfiguriert       |   Richtliniengesteuert, mit der `ColumnStoreTimeToLive`-Eigenschaft im Azure Cosmos-Container konfiguriert      |
|Preis pro GB    |   0,25 US-Dollar/GB      |  0,02 US-Dollar/GB       |
|Preis für Speichertransaktionen    | Der bereitgestellte Durchsatz wird mit 0,008 US-Dollar pro 100 RU/s im Stundentakt abgerechnet.        |  Der verbrauchsbasierte Durchsatz wird wie folgt abgerechnet: 0,05 US-Dollar pro 10.000 Schreibtransaktionen und 0,004 US-Dollar pro 10.000 Lesetransaktionen       |

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

### <a name="on-demand-snapshots-and-time-travel-analytics"></a>Bedarfsgesteuerte Momentaufnahmen und Time-Travel-Analysen

Sie können jederzeit Momentaufnahmen Ihrer Daten im analytischen Speicher des Azure Cosmos-Containers erstellen, indem Sie den `CreateSnapshot (name, timestamp)`-Befehl für den Container aufrufen. Momentaufnahmen werden im Verlauf aller für Ihren Container ausgeführten Aktualisierungen als „Lesezeichen“ bezeichnet.

![Bedarfsgesteuerte Momentaufnahmen und Time-Travel-Analysen](./media/globally-distributed-transactional-analytical-storage/ondemand-analytical-data-snapshots.png)

Bei der Erstellung der Momentaufnahme können Sie neben dem Namen den Zeitstempel angeben, der den Zustand des Containers im Aktualisierungsverlauf definiert. Anschließend können Sie die Momentaufnahmedaten in Spark laden und Abfragen ausführen.

Derzeit können nur bedarfsgesteuerte Momentaufnahmen vom Container erstellt werden. Die Möglichkeit, Momentaufnahmen automatisch anhand eines Zeitplans oder einer benutzerdefinierten Richtlinie zu erstellen, wird noch nicht unterstützt.

### <a name="configure-and-tier-data-between-transactional-and-analytical-storage-independently"></a>Unabhängiges Konfigurieren und Tiering von Daten zwischen dem transaktionalen und analytischen Speicher

Abhängig von Ihrem Szenario können Sie die beiden Speicher-Engines unabhängig voneinander aktivieren bzw. deaktivieren. Im folgenden die Konfigurationen für die einzelnen Szenarien:

|Szenario |Einstellung für transaktionalen Speicher  |Einstellung für analytischen Speicher |
|---------|---------|---------|
|Ausschließliche Ausführung analytischer Workloads (mit unbegrenzter Aufbewahrung) |  DefaultTimeToLive = 0       |  ColumnStoreTimeToLive = -1       |
|Ausschließliche Ausführung transaktionaler Workloads (mit unbegrenzter Aufbewahrung)  |   DefaultTimeToLive = -1      |  ColumnStoreTimeToLive = 0       |
|Ausführung transaktionaler und analytischer Workloads (mit unbegrenzter Aufbewahrung)   |   DefaultTimeToLive = -1      | ColumnStoreTimeToLive = -1        |
|Ausführung transaktionaler und analytischer Workloads (mit unterschiedlichen Aufbewahrungszeiträumen, auch Speichertiering genannt)  |  DefaultTimeToLive = <Value1>       |     ColumnStoreTimeToLive = <Value2>    |

1. **Konfigurieren des Containers ausschließlich für analytische Workloads (mit unbegrenzter Aufbewahrung)**

   Sie können Ihren Azure Cosmos-Container ausschließlich für analytische Workloads konfigurieren. Diese Konfiguration hat den Vorteil, dass für den transaktionalen Speicher keine Kosten anfallen. Wenn Sie den Container nur für analytische Workloads verwenden möchten, können Sie den transaktionalen Speicher deaktivieren, indem Sie `DefaultTimeToLive` für den Cosmos-Container auf 0 festlegen. Außerdem können Sie den analytischen Speicher mit unbegrenzter Aufbewahrung aktivieren, indem Sie `ColumnStoreTimeToLive` auf -1 festlegen.

   ![Analytische Workloads mit unbegrenzter Aufbewahrung](./media/globally-distributed-transactional-analytical-storage/analytical-workload-configuration.png)

1. **Konfigurieren des Containers ausschließlich für transaktionale Workloads (mit unbegrenzter Aufbewahrung)**

   Sie können Ihren Azure Cosmos-Container ausschließlich für transaktionale Workloads konfigurieren. Sie können den analytischen Speicher deaktivieren, indem Sie `ColumnStoreTimeToLive` für den Container auf 0 festlegen. Außerdem können Sie den analytischen Speicher mit unbegrenzter Aufbewahrung aktivieren, indem Sie `DefaultTimeToLive` auf -1 festlegen.

   ![Transaktionale Workloads mit unbegrenzter Aufbewahrung](./media/globally-distributed-transactional-analytical-storage/transactional-workload-configuration.png)

1. **Konfigurieren des Containers sowohl für transaktionale als auch für analytische Workloads (mit unbegrenzter Aufbewahrung)**

   Sie können Ihren Azure Cosmos-Container sowohl für transaktionale als auch für analytische Workloads konfigurieren, wobei die Leistung zwischen den Workloads vollständig isoliert wird. Sie können den analytischen Speicher aktivieren, indem Sie `ColumnStoreTimeToLive` auf -1 festlegen, und den transaktionalen Speicher mit unbegrenzter Aufbewahrung aktivieren, indem Sie `DefaultTimeToLive ` auf -1 festlegen.

   ![Transaktionale und analytische Workloads mit unbegrenzter Aufbewahrung](./media/globally-distributed-transactional-analytical-storage/analytical-transactional-configuration-infinite-retention.png)

1. **Konfigurieren des Containers sowohl für transaktionale als auch für analytische Workloads mit Speichertiering**

   Sie können Ihren Azure Cosmos-Container sowohl für transaktionale als auch für analytische Workloads konfigurieren, wobei die Leistung zwischen den Workloads mit unterschiedlichen Aufbewahrungszeiträumen vollständig isoliert wird. Azure Cosmos DB erzwingt, dass der analytische Speicher immer länger aufbewahrt wird als der transaktionale Speicher.

   Sie können den transaktionalen Speicher mit unbegrenzter Aufbewahrung aktivieren, indem Sie `DefaultTimeToLive` auf <Wert 1> festlegen, und den analytischen Speicher, indem Sie `ColumnStoreTimeToLive` auf <Wert 2> festlegen. Azure Cosmos DB erzwingt, dass <Wert 2> immer größer als <Wert 1> ist.

   ![Transaktionale und analytische Workloads mit Speichertiering](./media/globally-distributed-transactional-analytical-storage/analytical-transactional-configuration-specified-retention.png)

## <a name="next-steps"></a>Nächste Schritte

* [Gültigkeitsdauer in Azure Cosmos DB](time-to-live.md)
