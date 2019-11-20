---
title: Spaltenmuster in Azure Data Factory-Mappingdatenflüssen
description: Erstellen generalisierter Datentransformationsmuster unter Verwendung von Spaltenmustern in Azure Data Factory-Mappingdatenflüssen
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/21/2019
ms.openlocfilehash: 0c9a3c2ef05f4a11933ca7fc81c7c0f87a612293
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72789926"
---
# <a name="using-column-patterns-in-mapping-data-flow"></a>Verwenden von Spaltenmustern in Mappingdatenflüssen

In verschiedenen Mappingdatenfluss-Transformationen kann auf Vorlagenspalten mithilfe von Mustern (anstelle von hartcodierten Spaltennamen) verwiesen werden. Dieser Abgleich wird als *Spaltenmuster* bezeichnet. Sie können Muster definieren, um Spalten auf der Grundlage von Name, Datentyp, Stream oder Position abgleichen, anstatt exakte Feldnamen zu verwenden. Spaltenmuster sind in zwei Szenarien hilfreich:

* Wenn Sie eingehende Quellfelder häufig ändern – etwa im Falle von sich ändernden Spalten in Textdateien oder NoSQL-Datenbanken. Dieses Szenario wird als [Schemaabweichung](concepts-data-flow-schema-drift.md) bezeichnet.
* Wenn Sie einen Vorgang für eine große Gruppe von Spalten ausführen möchten, um beispielsweise alle Spalten, deren Spaltenname „total“ enthält, in einen Double-Wert umzuwandeln.

Spaltenmuster stehen aktuell für die Transformationen „Abgeleitete Spalte“, „Aggregieren“, „Auswählen“ und „Senke“ zur Verfügung.

## <a name="column-patterns-in-derived-column-and-aggregate"></a>Spaltenmuster in „Abgeleitete Spalte“ und „Aggregieren“

Wenn Sie ein Spaltenmuster in einer abgeleiteten Spalte oder auf der Registerkarte „Aggregate“ einer Aggregattransformation hinzufügen möchten, klicken Sie rechts neben einer vorhandenen Spalte auf das Plussymbol. Wählen Sie **Add column pattern** (Spaltenmuster hinzufügen) aus. 

![Spaltenmuster](media/data-flow/columnpattern.png "Spaltenmuster")

Verwenden Sie den [Ausdrucks-Generator](concepts-data-flow-expression-builder.md), um die Übereinstimmungsbedingung einzugeben. Erstellen Sie einen booleschen Ausdruck, um Spalten auf der Grundlage von `name`, `type`, `stream` und `position` der Spalte abzugleichen. Das Muster wirkt sich sowohl auf Spalten mit Datendrift als auch auf definierte Spalten aus, bei denen die Bedingung zutrifft (also „true“ zurückgegeben wird).

Die beiden Ausdrucksfelder unterhalb der Übereinstimmungsbedingung geben die neuen Namen und Werte der betroffenen Spalten an. Verwenden Sie `$$`, um auf den vorhandenen Wert des abgeglichenen Felds zu verweisen. Das linke Ausdrucksfeld definiert den Namen, das rechte den Wert.

![Spaltenmuster](media/data-flow/columnpattern2.png "Spaltenmuster")

Das obige Spaltenmuster gleicht jede Spalte vom Typ „Double“ ab und erstellt pro Übereinstimmung eine einzelne Aggregatspalte. Zur Benennung der neuen Spalte wird der Name der übereinstimmenden Spalte mit „_total“ verkettet. Der Wert der neuen Spalte ist die gerundete aggregierte Summe des vorhandenen Double-Werts.

Um zu überprüfen, ob die Übereinstimmungsbedingung korrekt ist, können Sie auf der Registerkarte **Untersuchen** das Ausgabeschema definierter Spalten überprüfen oder auf der Registerkarte **Datenvorschau** eine Momentaufnahme der Daten anzeigen. 

![Spaltenmuster](media/data-flow/columnpattern3.png "Spaltenmuster")

## <a name="rule-based-mapping-in-select-and-sink"></a>Regelbasierte Zuordnung in „Auswählen“ und „Senke“

Beim Zuordnen von Spalten in Quell- und Auswahltransformationen können Sie entweder eine feste Zuordnung oder regelbasierte Zuordnungen hinzufügen. Wenn Ihnen das Schema Ihrer Daten bekannt ist und Sie erwarten, dass bestimmte Spalten aus dem Quelldataset immer bestimmten statischen Namen entsprechen, verwenden Sie die feste Zuordnung. Erstellen Sie bei Verwendung flexibler Schemas mithilfe der regelbasierten Zuordnung einen Musterabgleich auf der Grundlage von `name`, `type`, `stream` und `position` der Spalten. Es kann eine beliebige Kombination aus festen und regelbasierten Zuordnungen verwendet werden. 

Klicken Sie zum Hinzufügen einer regelbasierten Zuordnung auf **Zuordnung hinzufügen**, und wählen Sie **Rule-based mapping** (Regelbasierte Zuordnung) aus.

![Regelbasierte Zuordnung](media/data-flow/rule2.png "Regelbasierte Zuordnung")

Geben Sie im linken Ausdrucksfeld Ihre boolesche Übereinstimmungsbedingung ein. Geben Sie im rechten Ausdrucksfeld das Zuordnungsziel für die übereinstimmende Spalte an. Verwenden Sie `$$`, um auf den vorhandenen Namen des abgeglichenen Felds zu verweisen.

Wenn Sie auf das nach unten zeigende Chevronsymbol klicken, können Sie eine RegEx-Zuordnungsbedingung angeben.

Klicken Sie auf das Brillensymbol neben einer regelbasierten Zuordnung, um die abgeglichenen definierten Spalten und deren Zuordnungsziel anzuzeigen.

![Regelbasierte Zuordnung](media/data-flow/rule1.png "Regelbasierte Zuordnung")

Im obigen Beispiel werden zwei regelbasierte Zuordnungen erstellt. In der ersten werden alle Spalten, die nicht „movie“ heißen, ihren vorhandenen Werten zugeordnet. Die zweite Regel verwendet einen regulären Ausdruck, um alle Spalten abzugleichen, die mit „movie“ beginnen, und ordnet sie der Spalte „movieId“ zu.

Sollte Ihre Regel mehrere identische Zuordnungen zur Folge haben, aktivieren Sie **Skip duplicate inputs** (Doppelte Eingaben überspringen) oder **Skip duplicate outputs** (Doppelte Ausgaben überspringen), um Duplikate zu vermeiden.

## <a name="pattern-matching-expression-values"></a>Ausdruckswerte für den Musterabgleich

* `$$` wird zur Laufzeit in den Namen oder Wert der jeweiligen Übereinstimmung umgewandelt.
* `name` stellt den Namen der einzelnen eingehenden Spalten dar.
* `type` stellt den Datentyp der einzelnen eingehenden Spalten dar.
* `stream` stellt den Namen dar, der dem jeweiligen Datenstrom oder der Transformation in Ihrem Flow zugeordnet ist.
* `position` ist die Ordinalposition von Spalten in Ihrem Datenfluss.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich ausführlicher über die [Ausdruckssprache](data-flow-expression-functions.md) von Mappingdatenflüssen für Datentransformationen.
* Verwenden Sie Spaltenmuster in der [Senkentransformation](data-flow-sink.md) und der [Auswahltransformation](data-flow-select.md) mit regelbasierter Zuordnung.
