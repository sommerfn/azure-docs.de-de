---
title: Azure Data Factory Mapping Data Flow – Spaltenmuster
description: Erstellen generalisierter Datentransformationsmuster mit Spaltenmustern von Azure Data Factory in Mappingdatenflüssen
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 535656f315f65ffb7aa241618fe9e73b8246b71f
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72027866"
---
# <a name="mapping-data-flows-column-patterns"></a>Mappingdatenflüsse – Spaltenmuster



Mehrere Azure Data Factory-Datenflusstransformationen unterstützen das Konzept von „Spaltenmustern“, sodass Sie Vorlagenspalten erstellen können, die auf Mustern anstelle von hartcodierten Spaltennamen basieren. Sie können dieses Feature im Ausdrucks-Generator verwenden, um Muster gemäß den Spalten für die Transformation zu definieren, sodass keine exakten, bestimmten Feldnamen erforderlich sind. Muster sind bei sich häufig ändernden eingehenden Quellfeldern hilfreich, insbesondere beim Ändern von Spalten in Textdateien oder NoSQL-Datenbanken. Diese Bedingung wird manchmal als „Schemaabweichung“ bezeichnet.

Diese Handhabung von „Flexibles Schema“ gibt es derzeit in den Transformationen des Typs „Abgeleitete Spalte“ und „Aggregieren“ sowie in den Auswahl- und Senkentransformationen als „regelbasierte Zuordnung“.

![Spaltenmuster](media/data-flow/columnpattern2.png "Spaltenmuster")

## <a name="column-patterns"></a>Spaltenmuster
Spaltenmuster sind sowohl in Schemaabweichungs- als auch in allgemeinen Szenarien nützlich. Sie eignen sich gut für Bedingungen, bei denen Sie nicht jeden Spaltennamen vollständig kennen. Sie können eine Musterübereinstimmung basierend auf dem Spaltennamen und Spaltendatentyp herstellen und einen Ausdruck für die Transformation erstellen, die diesen Vorgang für jedes Feld im Datenstrom ausführt, das Ihren `name` & `type`-Mustern entspricht.

Wählen Sie beim Hinzufügen eines Ausdrucks zu einer Transformation, die Muster akzeptiert, die Option „Spaltenmuster hinzufügen“ aus. Spaltenmuster ermöglichen den Abgleich von Schemaabweichungs-Spaltenmustern.

Geben Sie beim Erstellen von Vorlagenspaltenmustern im Ausdruck `$$` an, um einen Verweis auf jedes übereinstimmende Feld aus dem Eingabedatenstrom darzustellen.

Wenn Sie eine der RegEx-Funktionen des Ausdrucks-Generators verwenden möchten, können Sie anschließend mit $1, $2, $3 usw. auf die von Ihrem regulären Ausdruck abgeglichenen Teilmuster verweisen.

Ein Beispiel für ein Spaltenmusterszenario ist die Verwendung von SUMME mit einer Reihe von eingehenden Feldern. Die aggregierten Berechnungen von SUMME befinden sich in der Aggregat-Transformation. Sie können dann bei jeder Übereinstimmung der Feldtypen, die mit „Integer“ übereinstimmen, SUMME verwenden und dann mit $$ auf jede Übereinstimmung in Ihrem Ausdruck verweisen.

## <a name="match-columns"></a>Spaltenabgleich
![Spaltenmustertypen](media/data-flow/pattern2.png "Mustertypen")

Um Muster auf der Grundlage von Spalten zu erstellen, können Sie Spaltenname, Typ, Stream oder Position für den Abgleich heranziehen und eine beliebige Kombination dieser Optionen mit Ausdrucksfunktionen und regulären Ausdrücken verwenden.

![Spaltenposition](media/data-flow/position.png "Spaltenposition")

## <a name="rule-based-mapping"></a>Regelbasierte Zuordnung
Beim Zuordnen von Spalten in Quell-und Auswahltransformationen haben Sie die Möglichkeit, „Feste Zuordnung“ oder „Regelbasierte Zuordnung“ auszuwählen. Wenn Sie das Schema Ihrer Daten kennen und bestimmte Spalten aus dem Quell-DataSet erwarten, die mit bestimmten statischen Namen immer identisch sind, können Sie die feste Zuordnung verwenden. Wenn Sie aber mit flexiblen Schemas arbeiten, verwenden Sie die regelbasierte Zuordnung. Sie können mithilfe der vorstehend beschriebenen Regeln einen Musterabgleich erstellen.

![regelbasierte Zuordnung](media/data-flow/rule2.png "Regelbasierte Zuordnung")

Erstellen Sie Regeln mithilfe des Ausdrucks-Generators. Ihre Ausdrücke geben einen booleschen Wert zurück, um eine Übereinstimmung mit Spalten (TRUE) oder einen Ausschluss von Spalten (FALSE) anzugeben.

## <a name="pattern-matching-special-columns"></a>Musterabgleich für spezielle Spalten

* `$$` wird in den Namen jeder Übereinstimmung zur Entwurfszeit im Debugmodus und bei der Ausführung zur Laufzeit übersetzt.
* `name` stellt den Namen der einzelnen eingehenden Spalten dar.
* `type` stellt den Datentyp der einzelnen eingehenden Spalten dar.
* `stream` stellt den Namen dar, der den einzelnen Datenströmen oder Transformationen in Ihrem Flow zugeordnet ist.
* `position` ist die Ordinalposition von Spalten in Ihrem Datenfluss.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich ausführlicher über die [Ausdruckssprache](https://aka.ms/dataflowexpressions) des ADF-Mappingdatenflusses für Datentransformationen.
* Verwenden von Spaltenmustern in der [Senkentransformation](data-flow-sink.md) und [Auswählen einer Transformation](data-flow-select.md) mit regelbasierter Zuordnung
