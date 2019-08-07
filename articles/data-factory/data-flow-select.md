---
title: 'Azure Data Factory Mapping Data Flow: Auswahltransformation'
description: 'Azure Data Factory Mapping Data Flow: Auswahltransformation'
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 15c74637a2dc42ec44f582878b5505d94637cd7b
ms.sourcegitcommit: da0a8676b3c5283fddcd94cdd9044c3b99815046
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314214"
---
# <a name="azure-data-factory-mapping-data-flow-select-transformation"></a>Azure Data Factory-Mapping Data Flow: Auswahltransformation
[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Verwenden Sie diese Transformation für die Spaltenselektivität (Reduzierung der Anzahl von Spalten), um Aliase für Spalten und Datenstromnamen zuzuweisen und um Spalten neu anzuordnen.

## <a name="how-to-use-select-transformation"></a>Verwendung der Option „Select Transformation“ (Transformation auswählen)
Mit der Auswahltransformation können Sie einem gesamten Datenstrom oder Spalten in diesem Datenstrom einen Alias zuweisen, unterschiedliche Namen (Aliase) zuweisen und dann in Ihrem Datenfluss auf diese neuen Namen verweisen. Diese Transformation ist für Selbstverknüpfungsszenarien nützlich. Die Methode zum Implementieren einer Selbstverknüpfung in ADF-Datenfluss besteht darin, einen Datenstrom auszuwählen, ihn mit „Neue Verzweigung“ zu verzweigen und dann sofort danach eine Auswahltransformation hinzuzufügen. Dieser Datenstrom weist nun einen neuen Namen auf, mit dem Sie wieder eine Verknüpfung mit dem ursprünglichen Datenstrom herstellen können und dabei eine Selbstverknüpfung erstellen:

![Selbstverknüpfung](media/data-flow/selfjoin.png "Selbstverknüpfung")

Im obigen Diagramm befindet sich die Auswahltransformation ganz oben. Dem ursprünglichen Datenstrom wird der Alias „OrigSourceBatting“ zugewiesen. In der hervorgehobenen Verknüpfungstransformation darunter können Sie sehen, dass wir diesen SELECT-Aliasdatenstrom der Auswahltransformation als rechte Verknüpfung verwenden, sodass wir sowohl auf der linken als auch auf der rechten Seite (des inneren Joins) auf den gleichen Schlüssel verweisen können.

Die Auswahltransformation kann auch als Möglichkeit verwendet werden, die Auswahl von Spalten in Ihrem Datenfluss aufzuheben. Wenn in Ihrer Senke beispielsweise 6 Spalten definiert sind, Sie aber nur 3 bestimmte Spalten für die Transformation auswählen und dann zur Senke fließen lassen möchten, können Sie mit der Auswahltransformation nur diese 3 Spalten auswählen.

> [!NOTE]
> Sie müssen „Alle auswählen“ deaktivieren, um nur bestimmte Spalten auswählen zu können.

![Auswahltransformation](media/data-flow/select001.png "Alias auswählen")

## <a name="options"></a>Optionen
* In der Standardeinstellung für die Auswahl werden alle eingehenden Spalten einbezogen und die ursprünglichen Namen beibehalten. Sie können dem Datenstrom einen Alias zuweisen, indem Sie den Namen der Auswahltransformation festlegen.
* Wenn Sie einzelnen Spalten Aliase zuweisen möchten, deaktivieren Sie „Alle auswählen“, und verwenden Sie im unteren Bereich die Spaltenzuordnung.
* Wählen Sie die Option „Skip Duplicates“ (Duplikate überspringen) aus, um doppelte Spalten aus Eingabe- oder Ausgabemetadaten zu entfernen.

![Duplikate überspringen](media/data-flow/select-skip-dup.png "Duplikate überspringen")

## <a name="next-steps"></a>Nächste Schritte
* Nachdem Sie die Option „Select“ (Auswählen) verwendet haben, um Spalten umzubenennen, neu anzuordnen und Aliase zuzuweisen, müssen Sie die [Sink transformation (Senkentransformation)](data-flow-sink.md) verwenden, um Ihre Daten im Datenspeicher zu speichern.
