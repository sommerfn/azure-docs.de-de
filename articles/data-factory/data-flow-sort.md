---
title: 'Azure Data Factory Mapping-Datenfluss: Transformation zum Sortieren'
description: 'Azure Data Factory Mapping-Daten: Transformation zum Sortieren'
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: f9d2699c870de21daea9ce421c79f089c04b99fa
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029124"
---
# <a name="azure-data-factory-data-flow-sort-transformations"></a>Azure Data Factory-Datenfluss: Transformationen zum Sortieren



![Sortiereinstellungen](media/data-flow/sort.png "Sortieren")

Mit der Transformation zum Sortieren können Sie die eingehenden Zeilen im aktuellen Datenstrom sortieren. Die ausgehenden Zeilen aus der Transformation zum Sortieren folgen anschließend den von Ihnen festgelegten Sortierregeln. Sie können einzelne Spalten auswählen und mithilfe des Pfeilindikators neben jedem Feld auf- oder absteigend sortieren. Wenn Sie eine Spalte vor dem Anwenden der Sortierung ändern müssen, klicken Sie auf „Berechnete Spalten“, um den Ausdrucks-Editor zu starten. Sie haben dann die Möglichkeit, einen Ausdruck für den Sortiervorgang zu erstellen, statt einfach eine Spalte für die Sortierung anzuwenden.

## <a name="case-insensitive"></a>Groß-/Kleinschreibung nicht beachten
Sie können „Groß-/Kleinschreibung nicht beachten“ aktivieren, wenn die Groß-/Kleinschreibung beim Sortieren von Zeichenfolgen- oder Textfeldern ignoriert werden soll.

Die Option „Nur innerhalb von Partitionen sortieren“ nutzt die Spark-Datenpartitionierung. Durch die Sortierung eingehender Daten nur in jeder Partition können Datenflüsse partitionierte Daten sortieren, statt den gesamten Datenstrom zu sortieren.

Jede Sortierbedingung in der Transformation zum Sortieren kann neu angeordnet werden. Wenn Sie also eine Spalte in der Sortierreihenfolge weiter nach oben verschieben müssen, ziehen Sie diese Zeile mit der Maus, und verschieben Sie die Zeile in der Sortierliste nach oben oder unten.

Die Partitionierung hat Auswirkungen auf die Sortierung

ADF-Datenfluss wird für Spark-Big-Data-Cluster ausgeführt, deren Daten auf mehrere Knoten und Partitionen verteilt sind. Es ist wichtig, dass Sie dies beim Entwerfen Ihres Datenflusses beachten, wenn die Transformation zum Sortieren verwendet werden soll, um die Datensortierreihenfolge beizubehalten. Wenn Sie Ihre Daten in einer nachfolgenden Transformation neu partitionieren möchten, geht Ihre Sortierung möglicherweise aufgrund dieser Umverteilung von Daten verloren.

## <a name="next-steps"></a>Nächste Schritte

Nach dem Sortieren können Sie die [Transformation für das Aggregieren](data-flow-aggregate.md) verwenden.
