---
title: Speicher- und Parallelitätsgrenzwerte
description: Zeigen Sie die Speicher- und Parallelitätsgrenzwerte an, die den verschiedenen Leistungsstufen und Ressourcenklassen in Azure SQL Data Warehouse zugewiesen sind.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload-management
ms.date: 10/04/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 3611e61b303997a4291f4436403bb0a95e647e65
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73686033"
---
# <a name="memory-and-concurrency-limits-for-azure-sql-data-warehouse"></a>Speicher- und Parallelitätsgrenzwerte – Azure SQL Data Warehouse
Zeigen Sie die Speicher- und Parallelitätsgrenzwerte an, die den verschiedenen Leistungsstufen und Ressourcenklassen in Azure SQL Data Warehouse zugewiesen sind.  

## <a name="data-warehouse-capacity-settings"></a>Data Warehouse-Kapazitätseinstellungen
Die folgenden Tabellen zeigen die maximale Kapazität für das Data Warehouse auf unterschiedlichen Leistungsstufen. Informationen zum Ändern der Leistungsstufe finden Sie unter [Skalieren von Computerressourcen – Portal](quickstart-scale-compute-portal.md).

### <a name="service-levels"></a>Servicelevel

Die Servicelevel reichen von DW100c bis DW30000c.

| Leistungsstufe | Serverknoten | Verteilungen pro Serverknoten | Arbeitsspeicher pro Data Warehouse (GB) |
|:-----------------:|:-------------:|:------------------------------:|:------------------------------:|
| DW100c            | 1             | 60                             |    60                          |
| DW200c            | 1             | 60                             |   120                          |
| DW300c            | 1             | 60                             |   180                          |
| DW400c            | 1             | 60                             |   240                          |
| DW500c            | 1             | 60                             |   300                          |
| DW1000c           | 2             | 30                             |   600                          |
| DW1500c           | 3             | 20                             |   900                          |
| DW2000c           | 4             | 15                             |  1200                          |
| DW2500c           | 5             | 12                             |  1500                          |
| DW3000c           | 6             | 10                             |  1800                          |
| DW5000c           | 10            | 6                              |  3000                          |
| DW6000c           | 12            | 5                              |  3600                          |
| DW7500c           | 15            | 4                              |  4500                          |
| DW10000c          | 20            | 3                              |  6000                          |
| DW15000c          | 30            | 2                              |  9000                          |
| DW30000c          | 60            | 1                              | 18000                          |

Der maximale Servicelevel ist DW30000c mit 60 Serverknoten und einer Verteilung pro Serverknoten. Beispielsweise verarbeitet ein 600-TB-Data Warehouse mit DW30000c etwa 10 TB pro Serverknoten.

## <a name="concurrency-maximums-for-workload-groups"></a>Parallelitätshöchstwerte für Workloadgruppen
Mit der Einführung von Workloadgruppen (Link folgt) gilt das Konzept von Parallelitätsslots nicht mehr.  Ressourcen werden pro Anforderung auf der Grundlage eines Prozentsatzes zugeordnet und in der Definition der Workloadgruppe angegeben.  Trotz der Entfernung von Parallelitätsslots gibt es jedoch je nach Dienstebene eine Mindestanzahl von Ressourcen, die pro Abfrage benötigt werden.  In der folgenden Tabelle ist die minimale Menge an Ressourcen definiert, die pro Abfrage auf den einzelnen Dienstebenen und die damit verbundene Parallelität erforderlich ist. 

|Service Level|Maximale Anzahl gleichzeitiger Abfragen|Min. % unterstützt für REQUEST_MIN_RESOURCE_GRANT_PERCENT|
|---|---|---|
|DW100c|4|25%|
|DW200c|8|12,5 %|
|DW300c|12|8 %|
|DW400c|16|6,25 %|
|DW500c|20|5 %|
|DW1000c|32|3 %|
|DW1500c|32|3 %|
|DW2000c|48|2%|
|DW2500c|48|2%|
|DW3000c|64|1,5 %|
|DW5000c|64|1,5 %|
|DW6000c|128|0,75 %|
|DW7500c|128|0,75 %|
|DW10000c|128|0,75 %|
|DW15000c|128|0,75 %|
|DW30000c|128|0,75 %|
||||

## <a name="concurrency-maximums-for-resource-classes"></a>Parallelitätshöchstwerte für Ressourcenklassen
Um sicherzustellen, dass für jede Abfrage genügend Ressourcen zur effizienten Verarbeitung zur Verfügung stehen, verfolgt SQL Data Warehouse die Nutzung von Ressourcen, indem jeder Abfrage Parallelitätsslots zugewiesen werden. Das System stellt Abfragen basierend auf Wichtigkeit und Parallelitätsslots in eine Warteschlange. Abfragen verbleiben in der Warteschlange, bis genügend Parallelitätsslots verfügbar sind. [Wichtigkeit](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-workload-importance) und Parallelitätsslots legen außerdem die CPU-Prioritäten fest. Weitere Informationen finden Sie unter [Analysieren Ihrer Workload](analyze-your-workload.md).

**Statische Ressourcenklassen**

Die folgende Tabelle zeigt die maximale Anzahl gleichzeitiger Abfragen und die Parallelitätsslots für jede [statische Ressourcenklasse](resource-classes-for-workload-management.md).  

| Service Level | Maximale Anzahl gleichzeitiger Abfragen | Verfügbare Parallelitätsslots | Durch „staticrc10“ verwendete Slots | Durch „staticrc20“ verwendete Slots | Durch „staticrc30“ verwendete Slots | Durch „staticrc40“ verwendete Slots | Durch „staticrc50“ verwendete Slots | Durch „staticrc60“ verwendete Slots | Durch „staticrc70“ verwendete Slots | Durch „staticrc80“ verwendete Slots |
|:-------------:|:--------------------------:|:---------------------------:|:---------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|:----------:|
| DW100c        |  4                         |    4                        | 1         | 2          | 4          | 4          | 4         |  4         |  4         |  4         |
| DW200c        |  8                         |    8                        | 1         | 2          | 4          | 8          |  8         |  8         |  8         |  8        |
| DW300c        | 12                         |   12                        | 1         | 2          | 4          | 8          |  8         |  8         |  8         |   8        |
| DW400c        | 16                         |   16                        | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW500c        | 20                         |   20                        | 1         | 2          | 4          | 8          | 16         | 16         | 16         |  16        |
| DW1000c       | 32                         |   40                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW1500c       | 32                         |   60                        | 1         | 2          | 4          | 8          | 16         | 32         | 32         |  32        |
| DW2000c       | 48                         |   80                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW2500c       | 48                         |  100                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW3000c       | 64                         |  120                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         |  64        |
| DW5000c       | 64                         |  200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW6000c       | 128                        |  240                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW7500c       | 128                        |  300                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW10000c      | 128                        |  400                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW15000c      | 128                        |  600                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |
| DW30000c      | 128                        | 1200                        | 1         | 2          | 4          | 8          | 16         | 32         | 64         | 128        |

**Dynamische Ressourcenklassen**

Die folgende Tabelle zeigt die maximale Anzahl gleichzeitiger Abfragen und die Parallelitätsslots für jede [dynamische Ressourcenklasse](resource-classes-for-workload-management.md). Dynamische Ressourcenklassen verwenden für alle Servicelevel eine Speicherbelegung von 3-10-22-70 Prozent für die Ressourcenklassen small-medium-large-xlarge.

| Service Level | Maximale Anzahl gleichzeitiger Abfragen | Verfügbare Parallelitätsslots | Durch „smallrc“ verwendete Slots | Durch „mediumrc“ verwendete Slots | Durch „largerc“ verwendete Slots | Durch „xlargerc“ verwendete Slots |
|:-------------:|:--------------------------:|:---------------------------:|:---------------------:|:----------------------:|:---------------------:|:----------------------:|
| DW100c        |  4                         |    4                        | 1                     |  1                     |  1                    |   2                    |
| DW200c        |  8                         |    8                        | 1                     |  1                     |  1                    |   5                    |
| DW300c        | 12                         |   12                        | 1                     |  1                     |  2                    |   8                    |
| DW400c        | 16                         |   16                        | 1                     |  1                     |  3                    |  11                    |
| DW500c        | 20                         |   20                        | 1                     |  2                     |  4                    |  14                    |
| DW1000c       | 32                         |   40                        | 1                     |  4                     |  8                    |  28                    |
| DW1500c       | 32                         |   60                        | 1                     |  6                     |  13                   |  42                    |
| DW2000c       | 32                         |   80                        | 2                     |  8                     |  17                   |  56                    |
| DW2500c       | 32                         |  100                        | 3                     | 10                     |  22                   |  70                    |
| DW3000c       | 32                         |  120                        | 3                     | 12                     |  26                   |  84                    |
| DW5000c       | 32                         |  200                        | 6                     | 20                     |  44                   | 140                    |
| DW6000c       | 32                         |  240                        | 7                     | 24                     |  52                   | 168                    |
| DW7500c       | 32                         |  300                        | 9                     | 30                     |  66                   | 210                    |
| DW10000c      | 32                         |  400                        | 12                    | 40                     |  88                   | 280                    |
| DW15000c      | 32                         |  600                        | 18                    | 60                     | 132                   | 420                    |
| DW30000c      | 32                         | 1200                        | 36                    | 120                    | 264                   | 840                    |


Wenn nicht genügend Parallelitätsslots verfügbar sind, um die Abfrageausführung zu starten, werden Abfragen in die Warteschlange eingereiht und basierend auf ihrer Wichtigkeit ausgeführt.  Wenn die Wichtigkeit gleichwertig ist, werden Abfragen auf FIFO-Basis (First In, First Out) ausgeführt.  Wenn eine Abfrage abgeschlossen wird und die Anzahl von Abfragen und Slots unter die Grenzwerte sinkt, gibt SQL Data Warehouse Abfragen in der Warteschlange frei. 

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Verwenden von Ressourcenklassen zum weiteren Optimieren Ihrer Workload finden Sie in den folgenden Artikeln:
* [Ressourcenklassen für die Workloadverwaltung](resource-classes-for-workload-management.md)
* [Analysieren Ihrer Workload](analyze-your-workload.md)