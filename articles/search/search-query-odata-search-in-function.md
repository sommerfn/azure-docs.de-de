---
title: Referenz für die OData-Funktion „search.in“ – Azure Search
description: Hier finden Sie Informationen zur OData-Funktion „search.in“ in Azure Search-Abfragen.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
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
ms.openlocfilehash: a61291e547021077341a5f1b3db7422afa5b9440
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449982"
---
# <a name="odata-searchin-function-in-azure-search"></a>OData-Funktion `search.in` in Azure Search

Ein gängiges Szenario für [OData-Filterausdrücke](query-odata-filter-orderby-syntax.md) ist die Überprüfung, ob ein einzelnes Feld in den Dokumenten jeweils einem von vielen möglichen Werten entspricht. So wird in einigen Anwendungen beispielsweise zur Implementierung der [Einschränkung aus Sicherheitsgründen](search-security-trimming-for-azure-search.md) ein Feld, das eine oder mehrere Prinzipal-IDs enthält, mit einer Liste von Prinzipal-IDs verglichen, die den Benutzer darstellen, der die Abfrage ausführt. Eine solche Abfrage kann beispielsweise mithilfe der Operatoren [`eq`](search-query-odata-comparison-operators.md) und [`or`](search-query-odata-logical-operators.md) erstellt werden:

    group_ids/any(g: g eq '123' or g eq '456' or g eq '789')

Die Funktion `search.in` bietet hier eine kürzere Möglichkeit:

    group_ids/any(g: search.in(g, '123, 456, 789'))

> [!IMPORTANT]
> Diese Methode ist kürzer und besser lesbar. Darüber hinaus bietet die Verwendung der Funktion `search.in` auch [Leistungsvorteile](#bkmk_performance) und ermöglicht die Vermeidung bestimmter [Größeneinschränkungen von Filtern](search-query-odata-filter.md#bkmk_limits), wenn hunderte oder sogar tausende Werte in den Filter eingeschlossen werden. Aus diesem Grund empfehlen wir dringend, anstelle einer komplexeren Disjunktion von Gleichheitsausdrücken die Funktion `search.in` zu verwenden.

> [!NOTE]
> In der Version 4.01 des OData-Standards wurde kürzlich der [`in`-Operator](https://docs.oasis-open.org/odata/odata/v4.01/cs01/part2-url-conventions/odata-v4.01-cs01-part2-url-conventions.html#_Toc505773230) eingeführt, der sich ähnlich verhält wie die Funktion `search.in` in Azure Search. Dieser Operator wird von Azure Search allerdings nicht unterstützt, weshalb stattdessen die Funktion `search.in` verwendet werden muss.

## <a name="syntax"></a>Syntax

Die folgende EBNF ([Erweiterte Backus-Naur-Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) definiert die Grammatik der Funktion `search.in`:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_in_call ::=
    'search.in(' variable ',' string_literal(',' string_literal)? ')'
```

Ein interaktives Syntaxdiagramm ist ebenfalls verfügbar:

> [!div class="nextstepaction"]
> [OData-Syntaxdiagramm für Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#search_in_call)

> [!NOTE]
> Die vollständige EBNF finden Sie in der [Referenz zur OData-Ausdruckssyntax für Azure Search](search-query-odata-syntax-reference.md).

Die Funktion `search.in` testet, ob ein bestimmtes Zeichenfolgenfeld oder eine Bereichsvariable einem der Werte in einer angegebenen Liste entspricht. Die Gleichheit zwischen der Variablen und den einzelnen Werten in der Liste wird wie beim `eq`-Operator unter Berücksichtigung der Groß-/Kleinschreibung ermittelt. Ein Ausdruck wie `search.in(myfield, 'a, b, c')` entspricht daher `myfield eq 'a' or myfield eq 'b' or myfield eq 'c'`, allerdings bietet `search.in` eine deutlich bessere Leistung.

Es gibt zwei Überladungen der Funktion `search.in`:

- `search.in(variable, valueList)`
- `search.in(variable, valueList, delimiters)`

Die Parameter werden in der folgenden Tabelle definiert:

| Parametername | Typ | Beschreibung |
| --- | --- | --- |
| `variable` | `Edm.String` | Ein Verweis auf ein Zeichenfolgenfeld (oder eine Bereichsvariable für ein Zeichenfolgensammlungsfeld, falls `search.in` innerhalb eines Ausdrucks vom Typ `any` oder `all` verwendet wird). |
| `valueList` | `Edm.String` | Eine Zeichenfolge mit einer durch Trennzeichen getrennten Liste von Werten für den Abgleich mit dem Parameter `variable`. Ohne Angabe des Parameters `delimiters` werden standardmäßig Leerzeichen und Kommas als Trennzeichen verwendet. |
| `delimiters` | `Edm.String` | Eine Zeichenfolge, in der bei der Analyse des Parameters `valueList` jedes Zeichen als Trennzeichen behandelt wird. Der Standardwert dieses Parameters ist `' ,'`. Das bedeutet, dass alle Werte, zwischen denen sich Leerzeichen und/oder Kommas befinden, getrennt werden. Falls Sie anstelle von Leerzeichen und Kommas andere Trennzeichen verwenden müssen, da Ihre Werte diese Zeichen enthalten, können Sie in diesem Parameter alternative Trennzeichen angeben (beispielsweise `'|'`). |

<a name="bkmk_performance"></a>

### <a name="performance-of-searchin"></a>Leistung von `search.in`

Bei der Verwendung von `search.in` ist eine Antwortzeit von unter einer Sekunde zu erwarten, wenn der zweite Parameter eine Liste von Hunderten oder Tausenden von Werten enthält. Für die Anzahl von Elementen, die Sie an `search.in` übergeben können, gibt es keinen expliziten Grenzwert. Die maximal zulässige Anforderungsgröße gilt allerdings weiterhin. Die Wartezeit nimmt jedoch mit zunehmender Anzahl von Werten zu.

## <a name="examples"></a>Beispiele

Suche nach allen Hotels, deren Name „Sea View motel“ oder „Budget hotel“ entspricht. Ausdrücke enthalten Leerzeichen, die Standardtrennzeichen sind. Sie können ein alternatives Trennzeichen in einfachen Anführungszeichen als dritten Zeichenfolgenparameter angeben:  

    search.in(HotelName, 'Sea View motel,Budget hotel', ',')

Suche nach allen Hotels, deren Namen „Sea View motel“ oder „Budget hotel“ entspricht (getrennt durch „|“):

    search.in(HotelName, 'Sea View motel|Budget hotel', '|')

Suche nach allen Hotels mit Zimmern, die über das Tag „wifi“ oder „tub“ verfügen:

    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'wifi, tub')))

Suchen Sie eine Übereinstimmung mit Ausdrücken innerhalb einer Sammlung, z.B. „beheizte Handtuchhalter“ oder „Haartrockner inklusive“ in Tags.

    Rooms/any(room: room/Tags/any(tag: search.in(tag, 'heated towel racks,hairdryer included', ','))

Suche nach allen Hotels ohne das Tag „motel“ oder „cabin“:

    Tags/all(tag: not search.in(tag, 'motel, cabin'))

## <a name="next-steps"></a>Nächste Schritte  

- [Filter in Azure Search](search-filters.md)
- [Übersicht über die OData-Ausdruckssprache für Azure Search](query-odata-filter-orderby-syntax.md)
- [Referenz zur OData-Ausdruckssyntax für Azure Search](search-query-odata-syntax-reference.md)
- [Search Documents &#40;Azure Search Service REST API&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) (Suchen nach Dokumenten: REST-API für den Azure Search-Dienst)
