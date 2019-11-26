---
title: Verwenden der Neupartitionierung zur Optimierung von Azure Stream Analytics-Aufträgen
description: In diesem Artikel wird beschrieben, wie Sie Azure Stream Analytics Aufträge, die nicht parallelisiert werden können, mithilfe der Neupartitionierung optimieren.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 09/19/2019
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: bbea71464e8a1f4e93e510106d372257f155b0c6
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2019
ms.locfileid: "72935064"
---
# <a name="use-repartitioning-to-optimize-processing-with-azure-stream-analytics"></a>Verwenden der Neupartitionierung zur Optimierung der Verarbeitung mit Azure Stream Analytics

In diesem Artikel erfahren Sie, wie Sie mit der Neupartitionierung Ihre Azure Stream Analytics-Abfrage für Szenarien skalieren, die nicht vollständig [parallelisiert](stream-analytics-scale-jobs.md) werden können.

Die Verwendung der [Parallelisierung](stream-analytics-parallelization.md) ist in folgenden Situationen möglicherweise nicht möglich:

* Sie verfügen nicht über den Partitionsschlüssel für den Eingabestream.
* Ihre Quelle verteilt die Eingaben auf mehrere Partitionen, die später zusammengeführt werden müssen. 

## <a name="how-to-repartition"></a>Neupartitionierung

Eine Neupartitionierung oder „Umsortierung“ ist erforderlich, wenn Sie Daten in einem Stream verarbeiten, der nicht nach einem natürlichen Eingabeschema (z. B. nach der **Partitions-ID** für Event Hubs) horizontal partitioniert wird. Nach einer Neupartitionierung kann jeder Shard unabhängig verarbeitet werden, sodass Sie die Streamingpipeline linear skalieren können.

Verwenden Sie für die Neupartitionierung das Schlüsselwort **INTO** nach einer **PARTITION BY**-Anweisung in Ihrer Abfrage. Im folgenden Beispiel werden die Daten von **DeviceID** auf 10 Partitionen aufgeteilt (partitioniert). Durch die Erstellung eines Hashwerts aus der Geräte-ID (**DeviceID**) wird festgelegt, welche Partition welchen Teilstream akzeptieren soll. Die Daten werden für jeden partitionierten Datenstrom unabhängig geleert. Dabei wird davon ausgegangen, dass die Ausgabe partitionierte Schreibvorgänge unterstützt und über 10 Partitionen verfügt.

```sql
SELECT * 
INTO output
FROM input
PARTITION BY DeviceID 
INTO 10
```

In der folgenden Beispielabfrage werden zwei Streams mit neu partitionierten Daten verknüpft. Beim Verknüpfen von zwei Streams mit neu partitionierten Daten müssen diese Streams denselben Partitionsschlüssel und dieselbe Anzahl aufweisen. Das Ergebnis ist ein Stream, der über das gleiche Partitionsschema verfügt.

```sql
WITH step1 AS (SELECT * FROM input1 PARTITION BY DeviceID INTO 10),
step2 AS (SELECT * FROM input2 PARTITION BY DeviceID INTO 10)

SELECT * INTO output FROM step1 PARTITION BY DeviceID UNION step2 PARTITION BY DeviceID
```

Das Ausgabeschema muss mit dem Schemaschlüssel und der Anzahl im Stream identisch sein, damit jeder Teilstream unabhängig geleert werden kann. Der Stream kann auch vor dem Leeren zusammengeführt und mit einem anderen Schema neu partitioniert werden. Sie sollten diese Methode jedoch vermeiden, da sie die allgemeine Latenz bei der Verarbeitung und die Ressourcennutzung erhöht.

## <a name="streaming-units-for-repartitions"></a>Streamingeinheiten für Neupartitionierungen

Experimentieren Sie, und beobachten Sie die Ressourcennutzung Ihres Auftrags, um die genaue Anzahl der benötigten Partitionen zu ermitteln. Die Anzahl der [Streamingeinheiten](stream-analytics-streaming-unit-consumption.md) (SU) muss an die physischen Ressourcen angepasst werden, die für die einzelnen Partitionen benötigt werden. Im Allgemeinen sind für jede Partition sechs SUs erforderlich. Wenn dem Auftrag nicht genügend Ressourcen zugewiesen sind, wendet das System die Neupartitionierung nur an, wenn es für den Auftrag von Vorteil ist.

## <a name="repartitions-for-sql-output"></a>Neupartitionierungen für SQL-Ausgaben

Wenn Ihr Auftrag SQL-Datenbank für die Ausgabe verwendet, nutzen Sie die explizite Neupartitionierung, um die optimale Partitionsanzahl zum Maximieren des Durchsatzes zuzuordnen. Da SQL am besten mit acht Writern funktioniert, kann die Neupartitionierung des Flows auf acht vor dem Leeren (oder früher) die Auftragsleistung steigern. 

Wenn mehr als acht Eingabepartitionen vorliegen, ist das Erben des Eingabepartitionierungsschemas möglicherweise keine geeignete Option. Erwägen Sie die Verwendung von [INTO](/stream-analytics-query/into-azure-stream-analytics#into-shard-count) in Ihrer Abfrage, um die Zahl der Ausgabeschreiber explizit anzugeben. 

Das folgende Beispiel liest aus der Eingabe, unabhängig davon, ob sie natürlich partitioniert ist, und partitioniert den Datenstrom zehnfach entsprechend der DeviceID-Dimension neu und leert die Daten für die Ausgabe. 

```sql
SELECT * INTO [output] FROM [input] PARTITION BY DeviceID INTO 10
```

Weitere Informationen finden Sie unter [Azure Stream Analytics-Ausgabe an Azure SQL-Datenbank](stream-analytics-sql-output-perf.md).


## <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit Azure Stream Analytics](stream-analytics-introduction.md)
* [Nutzen der Parallelisierung von Abfragen in Azure Stream Analytics](stream-analytics-parallelization.md)
