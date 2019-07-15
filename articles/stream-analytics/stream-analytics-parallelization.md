---
title: Verwenden der Abfrageparallelisierung und Skalierung in Azure Stream Analytics
description: In diesem Artikel erfahren Sie, wie Sie Stream Analytics-Aufträge durch Konfiguration von Eingabepartitionen, Optimierung der Abfragedefinition und Festlegung von Auftragsstreamingeinheiten skalieren.
services: stream-analytics
author: JSeb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: 4fd862c2442d2637d799a1f690d5f0a091c80562
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449190"
---
# <a name="leverage-query-parallelization-in-azure-stream-analytics"></a>Nutzen der Parallelisierung von Abfragen in Azure Stream Analytics
Dieser Artikel veranschaulicht das Nutzen der Parallelisierung in Azure Stream Analytics. Erfahren Sie, wie Sie Stream Analytics-Aufträge durch Konfigurieren der Eingabe in Partitionen und Optimieren der Analysenabfragedefinition skalieren.
Als Voraussetzung sollten Sie mit dem Konzept der Streamingeinheiten vertraut sein, die unter [Verstehen und Anpassen von Streamingeinheiten](stream-analytics-streaming-unit-consumption.md) beschrieben werden.

## <a name="what-are-the-parts-of-a-stream-analytics-job"></a>Welche Teile hat ein Stream Analytics-Auftrag?
Eine Stream Analytics-Auftragsdefinition umfasst Eingaben, Abfrage und Ausgabe. Bei Eingaben handelt es sich um Eingaben, aus denen der Auftrag den Datenstrom liest. Bei der Abfrage wird die Datenstromeingabe transformiert, und der Auftrag sendet die Auftragsergebnisse an die Ausgabe.

Für einen Auftrag wird mindestens eine Eingabequelle für Datenstreaming benötigt. Die Datenstrom-Eingabequelle kann entweder ein Azure Event Hub oder ein Azure Blob Storage sein. Weitere Informationen finden Sie unter [Einführung in Azure Stream Analytics](stream-analytics-introduction.md) und [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).

## <a name="partitions-in-sources-and-sinks"></a>Partitionen in Quellen und Senken
Bei der Skalierung eines Stream Analytics-Auftrags werden die Partitionen in der Eingabe oder Ausgabe genutzt. Durch die Partitionierung können Daten basierend auf einem Partitionsschlüssel in Teilmengen unterteilt werden. Ein Vorgang, bei dem Daten verbraucht werden (z.B. einem Stream Analytics-Auftrag), können unterschiedliche Partitionen parallel genutzt und in diesen geschrieben werden. Dadurch erhöht sich der Durchsatz. 

### <a name="inputs"></a>Eingaben
Alle Azure Stream Analytics-Eingaben können Partitionierung nutzen:
-   Event Hub (Partitionsschlüssel muss explizit mit dem Schlüsselwort „PARTITION BY“ festgelegt werden)
-   IoT Hub (Partitionsschlüssel muss explizit mit dem Schlüsselwort „PARTITION BY“ festgelegt werden)
-   Blob Storage

### <a name="outputs"></a>Ausgaben

Bei der Arbeit mit Stream Analytics können Sie Partitionierung in den Ausgaben nutzen:
-   Azure Data Lake Store
-   Azure-Funktionen
-   Azure Table
-   Blob Storage (Partitionsschlüssel kann explizit festgelegt werden)
-   Cosmos DB (Partitionsschlüssel muss explizit festgelegt werden)
-   Event Hubs (Partitionsschlüssel muss explizit festgelegt werden)
-   IoT Hub (Partitionsschlüssel muss explizit festgelegt werden)
-   Service Bus
- SQL und SQL Data Warehouse mit optionaler Partitionierung: Weitere Informationen finden Sie auf der Seite [Ausgabe an Azure SQL-Datenbank](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-sql-output-perf).

Power BI unterstützt keine Partitionierung. Sie können die Eingabe aber dennoch partitionieren, wie in [diesem Abschnitt](#multi-step-query-with-different-partition-by-values) beschrieben. 

Weitere Informationen zu den Partitionen finden Sie in den folgenden Artikeln:

* [Event Hubs-Features im Überblick](../event-hubs/event-hubs-features.md#partitions)
* [Datenpartitionierung](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning)


## <a name="embarrassingly-parallel-jobs"></a>Hochgradig parallele Aufträge
Ein *hochgradig paralleler* Auftrag stellt das am stärksten skalierbare Szenario dar, das in Azure Stream Analytics zur Verfügung steht. Er verbindet eine Partition der Eingabe mit einer Instanz der Abfrage und einer Partition der Ausgabe. Für eine solche Parallelität gelten folgende Anforderungen:

1. Wenn Ihre Abfragelogik davon abhängig ist, dass derselbe Schlüssel durch dieselbe Abfrageinstanz verarbeitet wird, müssen Sie sicherstellen, dass die Ereignisse in derselben Partition Ihrer Eingabe aufgenommen werden. Bei Event Hubs oder IoT Hub bedeutet dies, dass für die Ereignisdaten der Wert **PartitionKey** festgelegt sein muss. Alternativ können Sie partitionierte Absender verwenden. Bei Blob Storage bedeutet dies, dass die Ereignisse an denselben Partitionsordner gesendet werden. Wenn es für Ihre Abfragelogik nicht erforderlich ist, dass derselbe Schlüssel von derselben Abfrageinstanz verarbeitet wird, können Sie diese Anforderung ignorieren. Ein Beispiel für diese Logik wäre eine einfache Auswahl-, Projekt- oder Filterabfrage.  

2. Nachdem die Daten auf der Eingabeseite angeordnet wurden, müssen Sie sicherstellen, dass die Abfrage partitioniert wird. Dazu müssen Sie in allen Schritten **Partition by** verwenden. Es sind mehrere Schritte zulässig, aber sie müssen alle durch denselben Schlüssel partitioniert werden. Unterhalb des Kompatibilitätsgrads 1.0 und 1.1 muss der Partitionierungsschlüssel auf **PartitionId** festgelegt werden, damit der Auftrag vollständige Parallelität aufweist. Bei Aufträgen mit einem Kompatibilitätsgrad von 1.2 und höher kann die benutzerdefinierte Spalte in den Eingabeeinstellungen als „Partitionsschlüssel“ angegeben werden, und der Auftrag wird dann automatisch parallelisiert, auch wenn keine PARTITION BY-Klausel vorhanden ist.

3. Die meisten unserer Ausgaben können Partitionierung nutzen. Wenn Sie jedoch einen Ausgabetyp verwenden, der keine Partitionierung unterstützt, wird Ihr Auftrag nicht vollständig parallel ausgeführt. Weitere Informationen finden Sie im [Abschnitt über Ausgaben](#outputs).

4. Die Anzahl von Eingabepartitionen muss mit der Anzahl von Ausgabepartitionen identisch sein. Die Blob Storage-Ausgabe kann Partitionen unterstützen und erbt das Partitionierungsschema der Upstream-Abfrage. Bei der Angabe eines Partitionsschlüssels für Blob Storage werden Daten pro Eingabepartition partitioniert, daher ist das Ergebnis trotzdem vollständig parallel. Im Folgenden werden Beispiele für Partitionswerte vorgestellt, die einen vollständig parallelen Auftrag ermöglichen:

   * 8 Event Hub-Eingabepartitionen und 8 Event Hub-Ausgabepartitionen
   * 8 Event Hub-Eingabepartitionen und Blob Storage-Ausgabe
   * 8 Event Hub-Eingabepartitionen und Blob Storage-Ausgabe, partitioniert nach einem benutzerdefinierten Feld mit beliebiger Kardinalität
   * 8 Blob Storage-Eingabepartitionen und Blob Storage-Ausgabe
   * 8 Blob Storage-Eingabepartitionen und 8 Event Hub-Ausgabepartitionen

In den folgenden Abschnitten werden einige Beispielszenarien für hochgradige Parallelität behandelt.

### <a name="simple-query"></a>Einfache Abfrage

* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Event Hub mit 8 Partitionen

Abfrage:

```SQL
    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100
```

Diese Abfrage stellt einen einfachen Filter dar. Daher ist keine Partitionierung der Eingabe erforderlich, die an den Event Hub gesendet wird. Beachten Sie, dass Aufträge mit einem niedrigeren Kompatibilitätsgrad als 1.2 die **PARTITION BY PartitionId**-Klausel enthalten müssen, damit die zweite, weiter oben genannte Anforderung erfüllt ist. Für die Ausgabe muss die Event Hub-Ausgabe im Auftrag so konfiguriert werden, dass der Partitionsschlüssel auf **PartitionId** festgelegt ist. Eine letzte Prüfung besteht darin, sicherzustellen, dass die Anzahl der Eingabepartitionen mit der Anzahl der Ausgabepartitionen identisch ist.

### <a name="query-with-a-grouping-key"></a>Abfrage mit einem Gruppierungsschlüssel

* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Blob Storage

Abfrage:

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Diese Abfrage enthält einen Gruppierungsschlüssel. Aus diesem Grund müssen die gruppierten Ereignisse an dieselbe Event Hub-Partition gesendet werden. Da in diesem Beispiel nach TollBoothID gruppiert wird, müssen wir sicher sein, dass TollBoothID als Partitionsschlüssel verwendet wird, wenn die Ereignisse an Event Hub gesendet werden. Anschließend können wir in ASA **Partition by PartitionId** verwenden, um von diesem Partitionsschema zu erben und vollständige Parallelisierung zu aktivieren. Da es sich bei der Ausgabe um Blob Storage handelt, muss laut der vierten Anforderung kein Wert für den Partitionsschlüssel konfiguriert werden.

## <a name="example-of-scenarios-that-are-not-embarrassingly-parallel"></a>Beispielszenarien *ohne* hochgradige Parallelität

Im vorherigen Abschnitt haben wir einige Szenarien mit hochgradiger Parallelität vorgestellt. In diesem Abschnitt werden Szenarien erläutert, in denen nicht alle Anforderungen hinsichtlich hochgradiger Parallelität erfüllt werden. 

### <a name="mismatched-partition-count"></a>Ungleiche Anzahl von Partitionen
* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Event Hub mit 32 Partitionen

In diesem Fall spielt es keine Rolle, um welche Abfrage es sich handelt. Wenn die Anzahl der Eingabepartitionen nicht der Anzahl der Ausgabepartitionen entspricht, weist die Topologie keine hochgradige Parallelität auf. Es kann jedoch dennoch ein gewisser Grad an Parallelisierung erreicht werden.

### <a name="query-using-non-partitioned-output"></a>Abfragen mit nicht partitionierter Ausgabe
* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Power BI

Die Power BI-Ausgabe unterstützt derzeit keine Partitionierung. Daher besteht in diesem Szenario keine hochgradige Parallelität.

### <a name="multi-step-query-with-different-partition-by-values"></a>Mehrstufige Abfrage mit unterschiedlichen Werten für „Partition by“
* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Event Hub mit 8 Partitionen

Abfrage:

```SQL
    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

Wie Sie sehen, wird im zweiten Schritt **TollBoothId** als Partitionierungsschlüssel verwendet. Dieser Schritt entspricht nicht dem ersten Schritt, und deshalb muss eine Umschichtung durchgeführt werden. 

In den vorherigen Beispielen werden einige Stream Analytics-Aufträge gezeigt, die einer hochgradig parallelen Topologie entsprechen (oder nicht entsprechen). Liegt eine Entsprechung mit dieser Topologie vor, können sie maximal skaliert werden. Für Aufträge, für die keines dieser Profile geeignet ist, werden bei zukünftigen Updates Leitfäden bezüglich Skalierungen zur Verfügung gestellt. Verwenden Sie bis dahin die allgemeinen Hinweise in den folgenden Abschnitten.

### <a name="compatibility-level-12---multi-step-query-with-different-partition-by-values"></a>Kompatibilitätsgrad 1.2: Mehrstufige Abfrage mit unterschiedlichen Werten für „PARTITION BY“ 
* Eingabe: Event Hub mit 8 Partitionen
* Ausgabe: Event Hub mit 8 Partitionen

Abfrage:

```SQL
    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

Kompatibilitätsgrad 1.2 ermöglicht die standardmäßige parallele Abfrageausführung. Beispielsweise wird die Abfrage aus dem vorherigen Abschnitt so lange partitioniert, wie die Spalte „TollBoothId“ als Eingabepartitionsschlüssel festgelegt ist. Die „PARTITION BY ParttionId“-Klausel ist nicht erforderlich.

## <a name="calculate-the-maximum-streaming-units-of-a-job"></a>Berechnen der maximal möglichen Streaming-Einheiten für einen Auftrag
Die Gesamtzahl der von einem Stream Analytics-Auftrag verwendbaren Streaming-Einheiten hängt von der Anzahl an Schritten in der für den Auftrag definierten Abfrage und der Anzahl an Partitionen für die einzelnen Schritte ab.

### <a name="steps-in-a-query"></a>Schritte einer Abfrage
Eine Abfrage kann einen oder mehrere Schritte umfassen. Jeder Schritt besteht aus einer Unterabfrage, die mit dem Schlüsselwort **WITH** definiert wird. Die Abfrage, die sich außerhalb des Schlüsselworts **WITH** befindet (nur eine Abfrage), wird ebenfalls als Schritt gezählt, beispielsweise die **SELECT**-Anweisung in der folgenden Abfrage:

Abfrage:

```SQL
    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )
    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId
```

Die Abfrage umfasst zwei Schritte.

> [!NOTE]
> Diese Abfrage wird später im vorliegenden Artikel ausführlicher erläutert.
>  

### <a name="partition-a-step"></a>Partitionieren eines Schritts
Für die Partitionierung eines Schrittes gelten die folgenden Voraussetzungen:

* Die Eingabequelle muss partitioniert sein. 
* Die **SELECT** -Anweisung der Abfrage muss aus einer partitionierten Eingabequelle lesen.
* Die Abfrage innerhalb des Schritts muss das Schlüsselwort **Partition by** aufweisen.

Wenn eine Abfrage partitioniert ist, werden die Eingabeereignisse verarbeitet und in separaten Partitionsgruppen aggregiert, und für jede einzelne Gruppe werden Ausgabeereignisse generiert. Wenn Sie ein kombiniertes Aggregat bevorzugen, müssen Sie einen zweiten, nicht partitionierten Schritt zum Aggregieren erstellen.

### <a name="calculate-the-max-streaming-units-for-a-job"></a>Berechnen der maximal möglichen Streaming-Einheiten für einen Auftrag
Alle nicht partitionierten Schritte zusammen können auf bis zu sechs Streamingeinheiten (SUs) für einen Stream Analytics-Auftrag zentral hochskaliert werden. Darüber hinaus können Sie 6 SUs für jede Partition in einem partitionierten Schritt hinzufügen.
In der nachstehenden Tabelle werden einige **Beispiele** angegeben.

| Abfragen                                               | Max. Anzahl von SUs für den Auftrag |
| --------------------------------------------------- | ------------------- |
| <ul><li>Die Abfrage umfasst einen Schritt.</li><li>Der Schritt ist nicht partitioniert.</li></ul> | 6 |
| <ul><li>Der Eingabedatenstrom ist in 16 Partitionen unterteilt.</li><li>Die Abfrage umfasst einen Schritt.</li><li>Der Schritt ist partitioniert.</li></ul> | 96 (6*16 Partitionen) |
| <ul><li>Die Abfrage umfasst zwei Schritte.</li><li>Keiner der Schritte ist partitioniert.</li></ul> | 6 |
| <ul><li>Der Eingabedatenstrom ist in 3 partitioniert.</li><li>Die Abfrage umfasst zwei Schritte. Der Eingabeschritt ist partitioniert, nicht jedoch der zweite.</li><li>Die <strong>SELECT</strong>-Anweisung liest aus der partitionierten Eingabe.</li></ul> | 24 (18 für partitionierte Schritte + 6 für nicht partitionierte Schritte) |

### <a name="examples-of-scaling"></a>Beispiele für eine Skalierung

Mit der folgenden Abfrage wird die Anzahl der Fahrzeuge berechnet, die in einem Zeitraum von drei Minuten eine Mautstation mit drei Mautstellen passieren. Diese Abfrage kann auf bis zu sechs SUs skaliert werden.

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Um weitere SUs für die Abfrage zu verwenden, müssen sowohl der Eingabedatenstrom als auch die Abfrage partitioniert werden. Da die Datenstrompartition auf „3“ festgelegt ist, kann die folgende geänderte Abfrage auf bis zu 18 SUs skaliert werden:

```SQL
    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
```

Wenn eine Abfrage partitioniert ist, werden die Eingabeereignisse verarbeitet und in separaten Partitionsgruppen aggregiert. Zudem werden für die einzelnen Gruppen Ausgabeereignisse generiert. Die Partitionierung kann zu unerwarteten Ergebnissen führen, wenn das Feld **GROUP BY** nicht den Partitionsschlüssel im Eingabedatenstrom enthält. Beispielsweise enthält das Feld **TollBoothId** in der vorherigen Abfrage nicht den Partitionsschlüssel von **Input1**. Das Ergebnis: Die Daten aus der Mautstation 1 können auf mehrere Partitionen verteilt werden.

Alle Partitionen von **Input1** werden separat von Stream Analytics verarbeitet. Folglich werden im selben rollierenden Fenster mehrere Einträge von der Anzahl der Autos für dieselbe Mautstation erstellt. Falls der Eingabepartitionsschlüssel nicht geändert werden kann, kann dieses Problem durch Hinzufügen eines nicht partitionierten Schritts zum partitionsübergreifenden Sammeln von Werten behoben werden, wie im folgenden Beispiel gezeigt wird:

```SQL
    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId
```

Diese Abfrage kann auf bis zu 24 SUs skaliert werden.

> [!NOTE]
> Wenn Sie zwei Datenströme verknüpfen, stellen Sie sicher, dass die Datenströme anhand des Partitionsschlüssels der Spalte partitioniert werden, in der Sie die Verknüpfungen vornehmen. Stellen Sie außerdem sicher, dass in beiden Datenströmen dieselbe Anzahl von Partitionen vorliegt.
> 
> 

## <a name="achieving-higher-throughputs-at-scale"></a>Erzielen eines bedarfsorientierten höheren Durchsatzes

Ein [hochgradig paralleler](#embarrassingly-parallel-jobs) Auftrag ist notwendig, aber nicht ausreichend, um bei Bedarf einen höheren Durchsatz aufrechtzuerhalten. Jedes Speichersystem und seine entsprechende Stream Analytics-Ausgabe enthält Variationen dabei, wie der beste Schreibdurchsatz zu erzielen ist. Wie bei jedem bedarfsorientierten Szenario gibt es einige Herausforderungen, die durch Einsatz der richtigen Konfigurationen bewältigt werden können. In diesem Abschnitt werden Konfigurationen für einige gängige Ausgaben erläutert und Beispiele für die Aufrechterhaltung der Datenerfassungsraten von 1 K, 5 K und 10 K Ereignissen pro Sekunde gezeigt.

Die folgenden Beobachtungen verwenden einen Stream Analytics-Auftrag mit zustandsloser (Pass-Through) Abfrage, eine einfache JavaScript-UDF, die in Event Hub, Azure SQL-DB oder Cosmos DB schreibt.

#### <a name="event-hub"></a>Event Hub

|Datenerfassungsrate (Ereignisse pro Sekunde) | Streaming-Einheiten | Ausgaberessourcen  |
|--------|---------|---------|
| 1 K     |    1    |  2 TU   |
| 5 K     |    6    |  6 TU   |
| 10 K    |    12   |  10 TU  |

Die [Event Hub](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-eventhubs)-Lösung lässt sich hinsichtlich der Streamingeinheitein (SU) und des Durchsatzes linear skalieren, was sie zur effizientesten und leistungsstärksten Methode zum Analysieren und Streamen von Daten aus Stream Analytics macht. Aufträge lassen sich auf bis zu 192 SU skalieren, was grob einem Verarbeitungsdurchsatz von bis zu 200 MB/s oder 19 Billionen Ereignissen pro Tag entspricht.

#### <a name="azure-sql"></a>Azure SQL
|Datenerfassungsrate (Ereignisse pro Sekunde) | Streaming-Einheiten | Ausgaberessourcen  |
|---------|------|-------|
|    1 K   |   3  |  S3   |
|    5 K   |   18 |  P4   |
|    10 K  |   36 |  P6   |

[Azure SQL](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-azuresql) unterstützt paralleles Schreiben, auch Partitionierungsvererbung genannt, was aber nicht standardmäßig aktiviert ist. Allerdings ist das Aktivieren der Partitionierungsvererbung zusammen mit einer vollständig parallelen Abfrage möglicherweise nicht ausreichend, um höhere Durchsätze zu erreichen. Die Schreibdurchsätze von SQL sind signifikant von Ihrer SQL Azure-Datenbankkonfiguration und dem Tabellenschema abhängig. Der Artikel [SQL-Ausgabeleistung](./stream-analytics-sql-output-perf.md) enthält weitere Details zu den Parametern, mit denen sich Ihr Schreibdurchsatz maximieren lässt. Wie im Artikel [Azure Stream Analytics-Ausgabe in Azure SQL-Datenbank](./stream-analytics-sql-output-perf.md#azure-stream-analytics) erwähnt, lässt sich diese Lösung nicht als vollständig parallele Pipeline linear über 8 Partitionen hinaus skalieren, und sie erfordert möglicherweise vor der SQL-Ausgabe eine Neupartitionierung (siehe unter [ INTO](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count)). Premium-SKUs sind erforderlich, um hohe E/A-Raten zusammen mit dem Mehraufwand durch Protokollsicherungen, die alle paar Minuten erfolgen, aufrechtzuerhalten.

#### <a name="cosmos-db"></a>Cosmos DB
|Datenerfassungsrate (Ereignisse pro Sekunde) | Streaming-Einheiten | Ausgaberessourcen  |
|-------|-------|---------|
|  1 K   |  3    | 20 K RU  |
|  5 K   |  24   | 60 K RU  |
|  10 K  |  48   | 120 K RU |

Die [Cosmos DB](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb)-Ausgabe von Stream Analytics wurde so aktualisiert, dass unterhalb des [Kompatibilitätsgrad 1.2](./stream-analytics-documentdb-output.md#improved-throughput-with-compatibility-level-12) native Integration verwendet wird. Kompatibilitätsgrad 1.2 ermöglicht einen wesentlich höheren Durchsatz und verringert den RU-Verbrauch im Vergleich zu 1.1, bei dem es sich um den Standardkompatibilitätsgrad für neue Aufträge handelt. Die Lösung verwendet unter „/deviceId“ partitionierte CosmosDB-Container, und der Rest der Lösung ist identisch konfiguriert.

Alle [Azure-Beispiele zum bedarfsorientierten Streaming](https://github.com/Azure-Samples/streaming-at-scale) verwenden einen Event Hub, der von Last simulierenden Testclients Eingaben erhält. Jedes Eingabeereignis ist ein 1-KB-JSON-Dokument, das problemlos konfigurierte Datenerfassungsraten in Durchsatzraten (1 MB/s, 5 MB/s und 10 MB/s) umsetzt. Ereignisse simulieren ein IoT-Gerät, das die folgenden JSON-Daten (in einer Kurzform) sendet für bis zu 1-K-Geräte:

```
{
    "eventId": "b81d241f-5187-40b0-ab2a-940faf9757c0",
    "complexData": {
        "moreData0": 51.3068118685458,
        "moreData22": 45.34076957651598
    },
    "value": 49.02278128887753,
    "deviceId": "contoso://device-id-1554",
    "type": "CO2",
    "createdAt": "2019-05-16T17:16:40.000003Z"
}
```

> [!NOTE]
> Die Konfigurationen können sich aufgrund der verschiedenen Komponenten in der Lösung ändern. Um eine genauere Schätzung zu erhalten, passen Sie die Beispiele Ihrem Szenario an.

### <a name="identifying-bottlenecks"></a>Identifizieren von Engpässen

Verwenden Sie den Bereich „Metriken“ in ihrem Azure Stream Analytics-Auftrag, um Engpässe in Ihrer Pipeline zu identifizieren. Überprüfen Sie **Eingabe-/Ausgabeereignisse** hinsichtlich des Durchsatzes sowie ["Wasserzeichenverzögerung"](https://azure.microsoft.com/blog/new-metric-in-azure-stream-analytics-tracks-latency-of-your-streaming-pipeline/) oder **Ereignisse im Rückstand**, um festzustellen, ob der Auftrag mit der Eingangsrate Schritt halten kann. Suchen Sie für Event Hub-Metriken nach **Gedrosselte Anforderungen**, und passen Sie die Schwellenwerteinheiten (TU) entsprechend an. Überprüfen Sie für Cosmos DB-Metriken **Max. genutzte RU/Sek. pro Partitionsschlüsselbereich** unter „Durchsatz“, um sicherzustellen, dass Ihre Partitionsschlüsselbereiche gleichmäßig genutzt werden. Überwachen Sie für Azure SQL-DB **Protokoll-E/A** und **CPU**.

## <a name="get-help"></a>Hier erhalten Sie Hilfe

Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: https://support.microsoft.com
[azure.event.hubs.developer.guide]: https://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301

