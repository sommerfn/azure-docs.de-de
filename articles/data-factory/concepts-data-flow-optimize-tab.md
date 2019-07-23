---
title: Azure Data Factory Mapping Data Flow – Registerkarte „Optimieren“
description: Optimieren von Azure Data Factory Mapping Data Flows mithilfe der Registerkarte „Optimieren“ mit Partitionseinstellungen
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 40023931b2a0b3788a583a5b5240e7916b187e34
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190639"
---
# <a name="mapping-data-flow-transformation-optimize-tab"></a>Registerkarte „Optimieren“ für Mapping Data Flow-Transformationen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Jede Datenflusstransformation verfügt über eine Registerkarte „Optimieren“. Die Registerkarte „Optimieren“ enthält optionale Einstellungen zum Konfigurieren von Partitionierungsschemas für Datenflüsse.

<img src="media/data-flow/opt001.png" width="800">

Die Standardeinstellung ist „Aktuelle Partitionierung verwenden“. Die aktuelle Partitionierung weist Azure Data Factory an, das native Partitionierungsschema der in Spark in Azure Databricks ausgeführten Datenflüsse zu verwenden. Im Allgemeinen ist dies die empfohlene Vorgehensweise.

Es gibt jedoch Fälle, in denen Sie möglicherweise die Partitionierung anpassen möchten. Wenn beispielsweise Ihre Transformationen in einer einzigen Datei im Lake ausgegeben werden sollen, wählen Sie auf der Registerkarte „Optimieren“ die Option „Einzelne Partition“ für die Partitionierung in der Sink-Transformation aus.

Ein weiterer Grund, warum Sie möglicherweise Kontrolle über die für Ihre Datentransformationen verwendeten Partitionierungsschemas ausüben möchten, ist die Leistung. Das Anpassen der Partitionierung von Daten bietet ein gewisses Maß an Kontrolle über die Verteilung Ihrer Daten auf Computeknoten und Datenstandortoptimierungen, die sowohl positive als auch negative Auswirkungen auf die gesamte Datenflussleistung haben können.

Wenn Sie die Partitionierung für eine Transformation ändern möchten, klicken Sie einfach auf die Registerkarte „Optimieren“, und aktivieren Sie das Optionsfeld „Partitionierung festlegen“. Daraufhin werden mehrere Optionen für die Partitionierung angezeigt. Die beste Methode zum Implementieren der Partitionierung richtet sich jeweils nach den Datenmengen, Kandidatenschlüsseln, Nullwerten und der Kardinalität. Die bewährte Methode besteht darin, mit der Standardpartitionierung zu beginnen und dann die verschiedenen Partitionierungsoptionen auszuprobieren. Sie können in der Pipeline im Debugmodus testen und dann die für jede Transformationsgruppierung aufgewendete Zeit sowie die Verwendung der Partition in der Ansicht „Überwachung“ anzeigen.

<img src="media/data-flow/opt002.png" width="600">

### <a name="round-robin"></a>Round-Robin

Roundrobin ist eine einfache Partition, die automatisch Daten gleichmäßig auf Partitionen verteilt. Verwenden Sie „Roundrobin“, wenn Sie nicht über gute Kandidaten verfügen, um eine solide, intelligente Partitionierungsstrategie zu implementieren. Sie können die Anzahl der physischen Partitionen festlegen.

### <a name="hash"></a>Hash

Azure Data Factory erzeugt einen Hash von Spalten, um einheitliche Partitionen zu erstellen, sodass Zeilen mit ähnlichen Werten in die gleiche Partition fallen. Testen Sie bei Verwendung der Option „Hash“ auf mögliche Partitionsungleichmäßigkeiten. Sie können die Anzahl der physischen Partitionen festlegen.

### <a name="dynamic-range"></a>Dynamischer Bereich

Bei dieser Option werden dynamische Spark-Bereiche basierend auf den von Ihnen angegebenen Spalten oder Ausdrücken verwendet. Sie können die Anzahl der physischen Partitionen festlegen. 

### <a name="fixed-range"></a>Fester Bereich

Sie müssen einen Ausdruck erstellen, der einen festen Bereich für Werte in Ihren partitionierten Datenspalten bereitstellt. Sie müssen einen fundierten Überblick über Ihre Daten haben, bevor Sie diese Option verwenden, um Partitionsungleichmäßigkeiten zu vermeiden. Die von Ihnen für den Ausdruck eingegebenen Werte werden als Teil einer Partitionsfunktion verwendet. Sie können die Anzahl der physischen Partitionen festlegen.

### <a name="key"></a>Schlüssel

Wenn Sie eine genaue Kenntnis der Kardinalität Ihrer Daten haben, kann die Schlüsselpartitionierung eine gute Partitionsstrategie darstellen. Die Schlüsselpartitionierung erstellt Partitionen für jeden eindeutigen Wert in der Spalte. Sie können die Anzahl der Partitionen nicht festlegen, weil die Anzahl auf den eindeutigen Werten in den Daten basiert.

## <a name="next-steps"></a>Nächste Schritte

[Anleitung zur Leistung und Optimierung der Mapping Data Flow-Funktion](concepts-data-flow-performance.md)
[Überwachen von Datenflüssen](concepts-data-flow-monitoring.md)
