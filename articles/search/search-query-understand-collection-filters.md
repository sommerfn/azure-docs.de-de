---
title: Grundlegendes zu OData-Sammlungsfiltern
titleSuffix: Azure Cognitive Search
description: Informationen dazu, wie OData-Sammlungsfilter in Abfragen der kognitiven Azure-Suche funktionieren.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 9a57e1d16b13d822b6f5b541a7f838b0dd3a69ad
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/23/2019
ms.locfileid: "72794394"
---
# <a name="understanding-odata-collection-filters-in-azure-cognitive-search"></a>Grundlegendes zu OData-Sammlungsfiltern in der kognitiven Azure-Suche

Um in der kognitiven Azure-Suche nach Sammlungsfeldern zu [filtern](query-odata-filter-orderby-syntax.md), können Sie die [Operatoren `any` und `all`](search-query-odata-collection-operators.md) zusammen mit **Lambdaausdrücken** verwenden. Lambdaausdrücke sind boolesche Ausdrücke, die auf eine **Bereichsvariable** verweisen. Die Operatoren `any` und `all` sind analog zu einer `for`-Schleife in den meisten Programmiersprachen, wobei die Bereichsvariable die Rolle der Schleifenvariablen übernimmt und der Lambdaausdruck als Schleifenkörper fungiert. Die Bereichsvariable übernimmt den „aktuellen“ Wert der Sammlung während der Iteration durch die Schleife.

Zumindest funktioniert es so theoretisch. In der Praxis implementiert die kognitive Azure-Suche Filter auf eine ganz andere Weise als `for`-Schleifen. Im Idealfall wäre dieser Unterschied für Sie unsichtbar, aber unter bestimmten Umständen ist er es nicht. Das Endergebnis ist, dass es Regeln gibt, denen Sie beim Schreiben von Lambdaausdrücken folgen müssen.

In diesem Artikel wird erläutert, warum es Regeln für Sammlungsfilter gibt, indem untersucht wird, wie die kognitiven Azure-Suche diese Filter ausführt. Wenn Sie erweiterte Filter mit komplexen Lambdaausdrücken schreiben, finden Sie diesen Artikel möglicherweise hilfreich, um ein Verständnis dafür zu entwickeln, was mit Filtern möglich ist und warum.

Informationen dazu, welche Regeln für Sammlungsfilter gelten (sowie Beispiele) finden Sie unter [Problembehandlung von OData-Sammlungsfiltern in der kognitiven Azure-Suche](search-query-troubleshoot-collection-filters.md).

## <a name="why-collection-filters-are-limited"></a>Warum Sammlungsfilter eingeschränkt sind

Es gibt drei Hauptgründe, warum nicht alle Filterfunktionen für alle Arten von Sammlungen unterstützt werden:

1. Nur bestimmte Operatoren werden für bestimmte Datentypen unterstützt. Es ist beispielsweise nicht sinnvoll, die booleschen Werte `true` und `false` mit `lt`, `gt` usw. zu vergleichen.
1. Die kognitive Azure-Suche unterstützt keine **korrelierte Suche** für Felder vom Typ `Collection(Edm.ComplexType)`.
1. Die kognitive Azure-Suche verwendet invertierte Indizes zum Ausführen von Filtern für alle Typen von Daten (einschließlich Sammlungen).

Der erste Grund ist nur eine Folge der Definition der OData-Sprache und des EDM-Typsystems. Die letzten beiden Gründe werden im weiteren Verlauf dieses Artikels näher erläutert.

## <a name="correlated-versus-uncorrelated-search"></a>Korrelierte Suche im Vergleich zu nicht korrelierter Suche

Wenn mehrere Filterkriterien auf eine Sammlung komplexer Objekte angewendet werden, sind die Kriterien **korreliert**, da sie sich auf *jedes Objekt in der Sammlung* beziehen. Der folgende Filter gibt beispielsweise Hotels zurück, die mindestens ein Luxuszimmer mit einem Preis von weniger als 100 haben:

    Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 100)

Wenn die Filterung *unkorreliert* wäre, kann der obige Filter Hotels zurückgeben, in denen ein Zimmer deluxe ist und ein anderes Zimmer einen Basispreis von weniger als 100 hat. Das wäre nicht sinnvoll, da beide Klauseln des Lambdaausdrucks für die gleiche Bereichsvariable gelten, nämlich für `room`. Aus diesem Grund sind solche Filter korreliert.

Bei der Volltextsuche gibt es jedoch keine Möglichkeit, auf eine bestimmte Bereichsvariable zu verweisen. Wenn Sie die feldbezogene Suche verwenden, um eine [vollständige Lucene-Abfrage](query-lucene-syntax.md) wie diese auszugeben:

    Rooms/Type:deluxe AND Rooms/Description:"city view"

erhalten Sie möglicherweise Hotels zurück, in denen ein Zimmer deluxe ist und für ein anderes Zimmer „city view“ (Aussicht auf die Stadt) in der Beschreibung erwähnt wird. Beispielsweise würde das folgende Dokument mit einer `Id` von `1` der Abfrage entsprechen:

```json
{
  "value": [
    {
      "Id": "1",
      "Rooms": [
        { "Type": "deluxe", "Description": "Large garden view suite" },
        { "Type": "standard", "Description": "Standard city view room" }
      ]
    },
    {
      "Id": "2",
      "Rooms": [
        { "Type": "deluxe", "Description": "Courtyard motel room" }
      ]
    }
  ]
}
```

Der Grund dafür ist, dass `Rooms/Type` sich auf alle analysierten Begriffe des Felds `Rooms/Type` im gesamten Dokument und ähnlich für `Rooms/Description` bezieht, wie in den folgenden Tabellen gezeigt.

So wird `Rooms/Type` für die Volltextsuche gespeichert:

| Begriff in `Rooms/Type` | Dokument-IDs |
| --- | --- |
| deluxe | 1, 2 |
| Standard | 1 |

So wird `Rooms/Description` für die Volltextsuche gespeichert:

| Begriff in `Rooms/Description` | Dokument-IDs |
| --- | --- |
| courtyard | 2 |
| city | 1 |
| garden | 1 |
| large | 1 |
| Motel | 2 |
| room | 1, 2 |
| Standard | 1 |
| suite | 1 |
| view | 1 |

Im Gegensatz zum obigen Filter, der im Wesentlichen besagt, dass „Dokumente ermittelt werden sollen, in denen `Type` für ein Zimmer gleich „Deluxe Room“ (Luxuszimmer) und `BaseRate` für **dieses gleiche Zimmer** kleiner als 100 ist“, besagt die Suchabfrage, dass „Dokumente ermittelt werden sollen, in denen `Rooms/Type` den Begriff „deluxe“ und `Rooms/Description` den Begriff „city view“ (Aussicht auf die Stadt) enthält. Es gibt kein Konzept für einzelne Zimmer, deren Felder im letzteren Fall korreliert werden können.

> [!NOTE]
> Wenn Sie möchten, dass der kognitiven Azure-Suche Unterstützung für die korrelierte Suche hinzugefügt wird, stimmen Sie bitte für [dieses Benutzerfeedbackelement](https://feedback.azure.com/forums/263029-azure-search/suggestions/37735060-support-correlated-search-on-complex-collections) ab.

## <a name="inverted-indexes-and-collections"></a>Invertierte Indizes und Sammlungen

Sie haben vielleicht bemerkt, dass es weitaus weniger Einschränkungen für Lambdaausdrücke bei komplexen Sammlungen gibt als bei einfachen Sammlungen wie `Collection(Edm.Int32)`, `Collection(Edm.GeographyPoint)` usw. Dies liegt daran, dass die kognitive Azure-Suche komplexe Sammlungen als tatsächliche Sammlungen von Unterdokumenten speichert, während einfache Sammlungen überhaupt nicht als Sammlungen gespeichert werden.

Betrachten Sie beispielsweise ein filterbares Zeichenfolgen-Sammlungsfeld wie `seasons` in einem Index für einen Onlinehändler. Einige in diesen Index hochgeladene Dokumente könnten wie folgt aussehen:

```json
{
  "value": [
    {
      "id": "1",
      "name": "Hiking boots",
      "seasons": ["spring", "summer", "fall"]
    },
    {
      "id": "2",
      "name": "Rain jacket",
      "seasons": ["spring", "fall", "winter"]
    },
    {
      "id": "3",
      "name": "Parka",
      "seasons": ["winter"]
    }
  ]
}
```

Die Werte des Felds `seasons` werden in einer Struktur namens **invertierter Index** gespeichert, die in etwa so aussieht:

| Begriff | Dokument-IDs |
| --- | --- |
| spring | 1, 2 |
| summer | 1 |
| fall | 1, 2 |
| winter | 2, 3 |

Diese Datenstruktur ist so konzipiert, dass sie eine Frage sehr schnell beantwortet: In welchen Dokumenten kommt ein bestimmter Begriff vor? Die Beantwortung dieser Frage funktioniert eher wie eine einfache Gleichheitsprüfung und nicht wie die Ausführung einer Schleife für eine Sammlung. Genau aus diesem Grund erlaubt die kognitive Azure-Suche für Zeichenfolgensammlungen nur `eq` als Vergleichsoperator innerhalb eines Lambdaausdrucks für `any`.

Aufbauend auf der Gleichheit untersuchen wir nun, wie es möglich ist, mehrere Gleichheitsüberprüfungen für die gleiche Bereichsvariable mit `or` zu kombinieren. Dies funktioniert dank Algebra und [der distributiven Eigenschaft von Quantifizierern](https://en.wikipedia.org/wiki/Existential_quantification#Negation). Der folgende Ausdruck:

    seasons/any(s: s eq 'winter' or s eq 'fall')

entspricht:

    seasons/any(s: s eq 'winter') or seasons/any(s: s eq 'fall')

und jeder der beiden `any`-Unterausdrücke kann effizient mithilfe des invertierten Index ausgeführt werden. Dank [dem Negationsgesetz von Quantifizierern](https://en.wikipedia.org/wiki/Existential_quantification#Negation) ist auch dieser Ausdruck:

    seasons/all(s: s ne 'winter' and s ne 'fall')

entspricht:

    not seasons/any(s: s eq 'winter' or s eq 'fall')

Deshalb ist es möglich, `all` mit `ne` und `and` zu verwenden.

> [!NOTE]
> Obwohl die Details über den Rahmen dieses Dokuments hinausgehen, erstrecken sich diese Prinzipien auch auf [Abstands- und Überschneidungstests für Sammlungen von räumlichen Punkten](search-query-odata-geo-spatial-functions.md). Deshalb kann in `any`:
>
> - `geo.intersects` nicht negiert werden
> - `geo.distance` muss mit `lt` oder `le` verglichen werden
> - Ausdrücke müssen mit `or` und nicht mit `and` kombiniert werden
>
> Die entgegengesetzten Regeln gelten für `all`.

Eine größere Vielfalt von Ausdrücken ist zulässig, wenn Sie nach Sammlungen von Datentypen filtern, die die Operatoren `lt`, `gt`, `le` und `ge` unterstützen, z.B. `Collection(Edm.Int32)`. Insbesondere können Sie sowohl `and` als auch `or` in `any` verwenden, solange die zugrunde liegenden Vergleichsausdrücke zu **Bereichsvergleichen** mit `and` kombiniert werden, die dann mit `or` weiter kombiniert werden. Diese Struktur von booleschen Ausdrücken wird als [disjunktive Normalform (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form) bezeichnet, auch bekannt als „ORs of ANDs“. Im Gegensatz dazu müssen Lambdaausdrücke für `all` für diese Datentypen in [konjunktiver Normalform (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form) vorliegen, auch bekannt als „ANDs of ORs“. Die kognitive Azure-Suche ermöglicht solche Bereichsvergleiche, da sie diese effizient mit invertierten Indizes ausführen kann, genau wie die schnelle Suche nach Zeichenfolgen ermöglicht wird.

Zusammenfassend sind dies die Faustregeln für das, was in einem Lambdaausdruck zulässig ist:

- Innerhalb von `any` sind *positive Überprüfungen* immer zulässig, etwa Gleichheit, Bereichsvergleiche, `geo.intersects` oder `geo.distance` im Vergleich zu `lt` oder `le` (sehen Sie „Nähe“ als Gleichheit an, wenn es um die Überprüfung der Entfernung geht).
- In `any` ist `or` immer zulässig. Sie können `and` nur für Datentypen, die Bereichsüberprüfungen ausdrücken können, und nur dann verwenden, wenn Sie ORs of ANDs (DNF) verwenden.
- Innerhalb von `all` sind die Regeln umgekehrt: Nur *negative Überprüfungen* sind zulässig, Sie können `and` immer verwenden, und Sie können `or` nur für Bereichsüberprüfungen verwenden, die als ANDs of ORs (CNF) ausgedrückt werden.

In der Praxis sind dies die Arten von Filtern, die Sie ohnehin am ehesten verwenden werden. Es ist trotzdem hilfreich, die Grenzen dessen zu verstehen, was möglich ist.

Spezifische Beispiele dafür, welche Arten von Filtern zulässig sind und welche nicht, finden Sie unter [Schreiben gültiger Sammlungsfilter](search-query-troubleshoot-collection-filters.md#bkmk_examples).

## <a name="next-steps"></a>Nächste Schritte  

- [Problembehandlung von OData-Sammlungsfiltern in der kognitiven Azure-Suche](search-query-troubleshoot-collection-filters.md)
- [Filter in der kognitiven Azure-Suche](search-filters.md)
- [Übersicht über die OData-Ausdruckssprache für die kognitive Azure-Suche](query-odata-filter-orderby-syntax.md)
- [Referenz zur OData-Ausdruckssyntax für die kognitive Azure-Suche](search-query-odata-syntax-reference.md)
- [Suchen von Dokumenten &#40;REST-API für die kognitive Azure-Suche&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
