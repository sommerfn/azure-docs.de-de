---
title: OData-Referenz zu „$orderby“ – Azure Search
description: OData-Sprachreferenz für die Syntax von „$orderby“ in Azure Search-Abfragen.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
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
ms.openlocfilehash: 1ced35dc73e6d596fbeda32590ab0b69df396c5c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081867"
---
# <a name="odata-orderby-syntax-in-azure-search"></a>OData-Syntax von „$orderby“ in Azure Search

 Mit dem [OData-Parameter **$orderby**](query-odata-filter-orderby-syntax.md) können Sie in Azure Search eine benutzerdefinierte Sortierreihenfolge auf Suchergebnisse anwenden. In diesem Artikel wird die Syntax von **$orderby** detailliert beschrieben. Weitere allgemeine Informationen zur Verwendung von **$orderby** beim Darstellen von Suchergebnissen finden Sie unter [Arbeiten mit Suchergebnissen in Azure Search](search-pagination-page-layout.md).

## <a name="syntax"></a>Syntax

Der Parameter **$orderby** akzeptiert eine durch Trennzeichen getrennte Liste von bis zu 32 **ORDER BY-Klauseln**. Die Syntax einer ORDER BY-Klausel wird in der folgenden [erweiterten Backus-Naur-Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form) (EBNF) veranschaulicht:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'
```

Ein interaktives Syntaxdiagramm ist ebenfalls verfügbar:

> [!div class="nextstepaction"]
> [OData-Syntaxdiagramm für Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#order_by_clause)

> [!NOTE]
> Die vollständige EBNF finden Sie unter [Referenz zur OData-Ausdruckssyntax für Azure Search](search-query-odata-syntax-reference.md).

Jede Klausel enthält Sortierkriterien, optional gefolgt von einer Sortierreihenfolge (`asc` = aufsteigend oder `desc` = absteigend). Wenn Sie keine Richtung angeben, wird die Standardeinstellung „asc“ (aufsteigend) verwendet. Bei den Sortierkriterien kann es sich entweder um den Pfad eines Felds vom Typ `sortable` oder um einen Aufruf der Funktion [`geo.distance`](search-query-odata-geo-spatial-functions.md) oder [`search.score`](search-query-odata-search-score-function.md) handeln.

Wenn für mehrere Dokumente die gleichen Sortierkriterien gelten und die Funktion `search.score` nicht verwendet wird (z. B. wenn Sie nach einem numerischen Feld vom Typ `Rating` sortieren und drei Dokumente eine Bewertung von 4 aufweisen), werden Verknüpfungen auf Grundlage des Dokumentergebnisses in absteigender Reihenfolge getrennt. Wenn Dokumentergebnisse identisch sind (z. B. wenn in der Anforderung keine Volltextsuchabfrage angegeben ist), dann ist die relative Sortierung der verknüpften Dokumente unbestimmt.

Sie können mehrere Sortierkriterien angeben. Die Reihenfolge der Ausdrücke bestimmt die endgültige Sortierreihenfolge. Die Syntax zum absteigenden Sortieren nach Suchergebnis, gefolgt von der Bewertung lautet beispielsweise `$orderby=search.score() desc,Rating desc`.

Die Syntax für `geo.distance` in **$orderby** entspricht der Syntax in **$filter**. Bei der Verwendung von `geo.distance` in **$orderby** muss das Feld, für das der Ausdruck gilt, vom Typ `Edm.GeographyPoint` und zudem `sortable` sein.

Die Syntax für `search.score` in **$orderby** ist `search.score()`. Die Funktion `search.score` akzeptiert keine Parameter.

## <a name="examples"></a>Beispiele

Sortieren von Hotels in aufsteigender Reihenfolge nach Basistarif:

    $orderby=BaseRate asc

Sortieren von Hotels in absteigender Reihenfolge nach Bewertung und anschließend in aufsteigender Reihenfolge nach Basistarif (bedenken Sie, dass die Standardreihenfolge aufsteigend ist):

    $orderby=Rating desc,BaseRate

Sortieren von Hotels in absteigender Reihenfolge nach Bewertung und anschließend in aufsteigender Reihenfolge nach der Entfernung von den angegebenen Koordinaten:

    $orderby=Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

Sortieren von Hotels in absteigender Reihenfolge nach Suchergebnis („search.score“) und Bewertung und anschließend in aufsteigender Reihenfolge nach der Entfernung von den angegebenen Koordinaten, wobei von zwei Hotels mit identischen Relevanzergebnissen und -bewertungen das nächstgelegene zuerst aufgeführt wird:

    $orderby=search.score() desc,Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Nächste Schritte  

- [Arbeiten mit Suchergebnissen in Azure Search](search-pagination-page-layout.md)
- [Übersicht über die OData-Ausdruckssprache für Azure Search](query-odata-filter-orderby-syntax.md)
- [Referenz zur OData-Ausdruckssyntax für Azure Search](search-query-odata-syntax-reference.md)
- [Search Documents &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Suchen nach Dokumenten: REST-API für den Azure Search-Dienst)
